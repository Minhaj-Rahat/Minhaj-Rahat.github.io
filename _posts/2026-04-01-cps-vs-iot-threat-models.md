---
title: 'CPS vs IoT: Same Thing, Different Threat Models?'
date: 2026-04-01
permalink: /posts/2026/04/cps-vs-iot-threat-models/
tags:
  - IoT Security
  - Cyber-Physical Systems
  - Threat Modeling
  - Research
---

If you work in security research, you've probably noticed that "cyber-physical systems" (CPS) and "Internet of Things" (IoT) get used almost interchangeably. A smart thermostat is IoT. An industrial control system is CPS. But what about a connected insulin pump? A smart grid sensor? A self-driving car? The boundaries blur fast.

NIST tackled this confusion head-on in [SP 1900-202](https://doi.org/10.6028/NIST.SP.1900-202), a 2019 publication that analyzed 31 CPS definitions and 30 IoT definitions across two decades of literature. Their conclusion: the two concepts have converged to the point where they are largely interchangeable. But the *security implications* of that convergence are worth unpacking, because the threat models are not the same.

## Where they came from

CPS and IoT emerged from different communities with different priorities.

**CPS** was coined by Helen Gill at the NSF in 2006. It came from the computer science and systems engineering world, with roots in mechatronics, embedded systems, and control theory. The emphasis was always on the tight coupling between computation and physical processes: sensing the physical world, computing a response, and actuating a change. Think autopilot systems, robotic surgery, power grid controllers. The six characteristics that appear consistently across CPS definitions are: hybrid physical/logical systems, hybrid analytical methods, control, component classes (sensors, actuators, compute), time constraints, and trustworthiness (safety, reliability, security).

**IoT** was coined by Kevin Ashton in 1999, emerging from the RFID and supply chain community. It started with a much simpler idea: putting a tag on a physical object so you can track it. Over time, IoT expanded from trackable objects (an RFID-tagged package) to data objects (a temperature sensor) to interactive objects (a smart plug that responds to commands) to smart objects (a device that processes data locally and acts on it). By the mid-2010s, IoT definitions had evolved to look almost identical to CPS definitions.

## NIST's unified perspective

The NIST publication analyzed 11 papers that tried to distinguish CPS from IoT. The proposed distinctions fell into four categories: control (CPS controls physical processes, IoT just monitors), platform (CPS uses dedicated hardware, IoT uses commodity devices), internet (IoT requires internet connectivity, CPS doesn't), and human interaction (IoT is consumer-facing, CPS is industrial).

NIST found that none of these distinctions hold up reliably. Modern IoT devices absolutely perform control (a smart thermostat adjusts temperature). CPS systems use the internet (connected cars, telemedicine). The lines are artificial.

Their unified model defines CPS/IoT systems as having four component categories: logical (software, algorithms), physical (mechanical, electrical), transducing (sensors and actuators that bridge logical and physical), and human (operators, users). Any system with these interacting components, engineered for function through integrated logic and physics, qualifies as both CPS and IoT.

## Why the threat models diverge

So if CPS and IoT are conceptually the same, why does it matter which label you use? Because the label signals which *threat model* applies, and that changes what defenders need to worry about.

**IoT threat model: scale, diversity, weak defaults.** When people say "IoT security," they usually mean millions of cheap consumer devices with default passwords, no update mechanism, unencrypted storage, and firmware that hasn't been patched since manufacture. The attack surface is breadth. Mirai proved this in 2016: scan the internet for devices with default credentials, compromise them at scale, build a botnet. The attacker doesn't care about any individual device. The threats are malware propagation, botnet recruitment, data exfiltration, and lateral movement into home or enterprise networks.

This is the world my research lives in. In [CIAO](/posts/2025/10/ciao-cross-architecture-iot-malware/), we classified IoT malware families across ARM and MIPS architectures. In [Cimalir](/posts/2024/02/cimalir-cross-platform-malware-clustering/), we clustered malware that reuses code across families. In our [firmware security study](/posts/2024/02/evil-maids-iot-firmware-security/), we found that 13 out of 16 consumer IoT devices didn't encrypt data at rest, and 11 were vulnerable to firmware modification. These are IoT-style vulnerabilities: mass-produced devices with security features that exist in hardware but were never enabled.

**CPS threat model: targeted attacks on physical processes.** When people say "CPS security," they usually mean an adversary who targets a specific physical process for disruption or damage. Stuxnet destroyed centrifuges. The Ukraine power grid attacks caused blackouts. The Oldsmar water treatment hack attempted to raise sodium hydroxide levels to dangerous concentrations. The attacker cares deeply about the specific system. The threats are manipulation of sensor readings, unauthorized actuation, violation of timing constraints, and safety-critical failures.

The key difference is not technical; it's about attacker motivation and impact. An IoT botnet wants your smart camera for its bandwidth. A CPS attack wants your industrial controller for its ability to open a valve or trip a breaker.

**The dangerous middle ground.** The convergence NIST describes creates a category of systems that face *both* threat models simultaneously. A connected insulin pump is an IoT device (commodity hardware, internet-connected, potentially with weak defaults) that is also a CPS (it controls insulin delivery, a safety-critical physical process). A smart grid sensor is mass-deployed like IoT but monitors critical infrastructure like CPS. An autonomous vehicle is consumer-facing IoT running safety-critical CPS control loops.

These hybrid systems inherit the worst of both worlds: the broad attack surface of IoT (internet-facing, default credentials, unpatched firmware) combined with the high-consequence impact of CPS (physical harm, infrastructure disruption, safety failures).

## What this means for security research

If you're building defenses for these converging systems, a few implications stand out:

**Malware analysis needs to account for physical impact.** Most IoT malware research (including mine) focuses on classification, clustering, and detection. But as IoT devices increasingly control physical processes, we need to think about what a compromised device can *do* in the physical world, not just what malware family infected it.

**Firmware security is a CPS problem.** Our finding that most IoT devices don't implement secure boot or data encryption isn't just a data privacy issue. If those devices control physical actuators (locks, valves, motors), the lack of firmware integrity checking means an attacker can make the device do anything within its physical capabilities. That's a CPS threat realized through an IoT vulnerability.

**Cross-architecture analysis matters more as CPS/IoT converge.** CPS traditionally ran on specialized hardware with proprietary instruction sets. IoT runs on commodity ARM and MIPS chips. As these worlds merge, the same malware might target both a consumer smart plug and an industrial sensor running the same SoC. Tools that work across architectures, like the IR-based analysis in CIAO and Cimalir, become more important.

## Suggested reading

If you want to go deeper on the CPS/IoT relationship and what it means for security:

- **[NIST SP 1900-202: Cyber-Physical Systems and Internet of Things](https://doi.org/10.6028/NIST.SP.1900-202)** by Greer, Burns, Wollman, and Griffor (2019). The foundational document for understanding the CPS/IoT convergence. Covers the history of both terms, analyzes 61 definitions, proposes a unified components and interactions model, and discusses implications for design assurance and cyber-physical security. Free to download.

- **[NIST Cybersecurity for IoT Program](https://www.nist.gov/programs-projects/nist-cybersecurity-iot-program)** for current NIST efforts on IoT security standards and guidelines.

- **[NIST CPS Framework (SP 1500-201)](https://doi.org/10.6028/NIST.SP.1500-201)** for the broader CPS reference architecture, including the Trustworthiness Aspect that covers security, privacy, safety, reliability, and resilience.

---

*My research at UC Colorado Springs focuses on the IoT side of this convergence: cross-architecture malware analysis, firmware security, and ML-based malware classification. If you're working on CPS/IoT security or thinking about how these threat models intersect, I'd like to hear from you. Find me on [LinkedIn](https://linkedin.com/in/minhajul-alam-rahat-1aa157157) or email me at mrahat@uccs.edu.*
