---
title: 'ESIL Explained: The Rosetta Stone for Cross-Architecture Binary Analysis'
date: 2025-11-15
permalink: /posts/2025/11/esil-rosetta-stone-binary-analysis/
tags:
  - Reverse Engineering
  - Binary Analysis
  - radare2
  - Tutorial
---

If you've ever tried to compare a malware binary compiled for ARM with one compiled for MIPS, you've hit the wall: completely different opcodes, different register names, different instruction formats. It's like trying to compare a book written in Japanese with one written in Arabic, you can't do character-by-character matching, because the alphabets have nothing in common.

But what if you could translate both books into English first?

That's essentially what ESIL does for binary analysis. It's the secret weapon behind our malware classifier [CIAO](/posts/2025/10/ciao-cross-architecture-iot-malware/), and in this post I'll explain how it works, why it matters, and how you can start using it yourself.

## What is ESIL?

ESIL stands for **Evaluable Strings Intermediate Language**. It's radare2's intermediate representation — a way to express what any instruction *does* in a common format, regardless of which CPU it was written for.

Here's the core idea. Consider this simple operation: loading a value from memory into a register. On MIPS, it looks like this:

```
lw v0, 0x878(fp)
```

This loads a 4-byte word from the address `fp + 0x878` into register `v0`. On ARM, the same operation would use completely different syntax — `LDR`, different register names, different addressing modes.

But in ESIL, the MIPS instruction above becomes:

```
0x878,fp,+,[4],v0,=
```

Read it right-to-left (it's a stack-based language): *take 0x878, add it to fp, read 4 bytes from that address, store in v0.* The opcode `lw` is gone. What remains is pure semantics — what the instruction *does*, not *how* a specific CPU encodes it.

## A real example: two architectures, one ESIL

Let's look at a simple operation that appears in almost every function: loading a value from memory. Here's what it looks like on each architecture:

**MIPS:**
```
lw     v0, 0x878(fp)     ; load 4 bytes from [fp + 0x878] into v0
```

**ARM:**
```
ldr    r0, [fp, #0x878]  ; load 4 bytes from [fp + 0x878] into r0
```

Different opcodes (`lw` vs `ldr`), different register names (`v0` vs `r0`), different syntax for the memory offset. But both do the same thing: read 4 bytes from an address computed as frame pointer + offset.

In ESIL, both become something like:

```
0x878,fp,+,[4],<REG>,=
```

After normalization (replacing architecture-specific register names with `<REG>`), the ESIL is identical. The opcodes are gone. What remains is pure semantics.

The same convergence happens at scale. The CIAO paper's Figure 1 shows a full `average(a, b)` function lifted through Ghidra's P-Code — the operations `INT_ADD`, `INT_SDIV`, `COPY`, `RETURN` are the same for both x86 and ARM. ESIL achieves the same effect with a more compact, stack-based syntax that's easier to tokenize for ML pipelines.

## Why this matters for malware analysis

IoT malware authors compile the same source code for ARM, MIPS, x86, PowerPC, and more — all to infect different devices. A single botnet like Mirai ships binaries for 5+ architectures. If your analysis tools only work on one instruction set at a time, you need separate models, separate features, and separate training data for each architecture. That doesn't scale.

ESIL lets us build **one model that works across all architectures**. Here's what the pipeline looks like in CIAO:

```
ARM binary ─────┐                    ┌─── Function embeddings
                 ├─→ radare2 ESIL ─→ │    (shared vector space)
MIPS binary ────┘    + normalize     └─── same semantics = 
                                          similar vectors
```

The key steps:

1. **Disassemble** with radare2 and convert each instruction to ESIL
2. **Normalize** — replace general-purpose register names with `<REG>`, function calls with `<FUNC>`, constants with `<IMM>`, memory addresses with `<MEM>`
3. **Tokenize** — treat each ESIL expression as a sentence, with operators and operands as tokens
4. **Embed** — train Word2Vec on the token sequences so semantically equivalent tokens from different architectures land near each other in vector space
5. **Aggregate** — feed the token embeddings through a transformer encoder to produce a single embedding per function

After normalization, CIAO's entire vocabulary is just **130 tokens** — compared to 975 tokens when using raw opcodes. A smaller vocabulary means less noise, faster training, and better generalization.

## Try it yourself in 30 seconds

If you have radare2 installed, you can see ESIL for any binary right now:

```bash
$ r2 -q -c 'aaa; pdf @ main' your_binary
```

Or to see just the ESIL column:

```bash
$ r2 -q -c 'aaa; e asm.esil=true; pdf @ main' your_binary
```

You'll see each instruction followed by its ESIL equivalent. Try it on the same source compiled for two different architectures — you'll see the ESIL converge even as the assembly diverges.

For a Python-based workflow (which is what CIAO uses), you can access ESIL through `r2pipe`:

```python
import r2pipe

r2 = r2pipe.open("malware_sample.elf")
r2.cmd("aaa")  # analyze

# Get functions
functions = r2.cmdj("aflj")

for func in functions:
    # Get ESIL for each instruction in the function
    instructions = r2.cmdj(f"pdfj @ {func['offset']}")
    for inst in instructions.get("ops", []):
        esil = inst.get("esil", "")
        print(f"{inst.get('opcode', ''):30s}  →  {esil}")
```

## From ESIL to embeddings to classification

Once you have ESIL for every function, the normalization step is straightforward string replacement:

```python
def normalize_esil(esil_expr, special_regs={"sp", "pc", "gp"}):
    tokens = esil_expr.split(",")
    normalized = []
    for token in tokens:
        if token in special_regs:
            normalized.append(token)  # keep special registers
        elif is_register(token):
            normalized.append("<REG>")
        elif is_function_call(token):
            normalized.append("<FUNC>")
        elif is_constant(token):
            normalized.append("<IMM>")
        elif is_memory_address(token):
            normalized.append("<MEM>")
        else:
            normalized.append(token)  # keep ESIL operators as-is
    return ",".join(normalized)
```

After normalization, the Word2Vec + transformer pipeline produces function embeddings that live in a shared semantic space. Two functions compiled from the same source for different CPUs end up as nearby vectors — which is exactly what we need for the downstream GNN to learn family-specific patterns from Function Call Graphs.

## ESIL vs. other IRs

ESIL isn't the only game in town. Here's how it compares to alternatives we've used:

| IR | Tool | Strengths | We used it in |
|----|------|-----------|---------------|
| **ESIL** | radare2 | Fast, compact syntax, easy to tokenize for ML | CIAO |
| **P-Code** | Ghidra | Richer semantics, better decompilation | Cimalir |
| **LLVM IR** | RetDec, McSema | Closest to source-level, optimizable | (not yet) |
| **VEX** | angr/Valgrind | Well-suited for symbolic execution | (not yet) |

We chose ESIL for CIAO because of its faster processing time and compact representation — important when you're processing 9,954 malware samples. For Cimalir, where we needed richer function attributes (caller/callee counts, control transfers), Ghidra's P-Code was the better fit.

The broader lesson: **the choice of IR depends on what you're optimizing for.** Speed and vocabulary size? ESIL. Semantic depth? P-Code or LLVM IR. Symbolic execution? VEX. There's no universal winner.

## The bigger picture

Intermediate representations are a quiet superpower in binary analysis. They let you write tools that work across architectures without the combinatorial explosion of handling each ISA separately. Whether you're building a malware classifier, a vulnerability scanner, or a binary similarity engine, the first question is always: *at what level of abstraction should I analyze?*

For us, ESIL + normalization + transformer embeddings turned out to be the right recipe for cross-architecture malware classification. But the same principle, translate first, then analyze, applies far beyond malware.

If you want to dive deeper, check out the [CIAO paper](/posts/2025/10/ciao-cross-architecture-iot-malware/) and the [source code](https://github.com/ssluc/CIAO). The ESIL documentation is at [radare2's book](https://book.rada.re/esil.html).

---

*This is part of my PhD research at UC Colorado Springs. If you're working with intermediate representations for binary analysis or security, whether ESIL, P-Code, LLVM IR, or something else, I'd love to hear about your experience. Reach out on [LinkedIn](https://linkedin.com/in/minhajul-alam-rahat-1aa157157) or email me at mrahat@uccs.edu.*
