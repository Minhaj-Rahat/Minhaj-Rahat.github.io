---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

A PDF version of my CV is available [here](https://drive.google.com/file/d/1ccTC1QRXtKAMNLk0s6uXfsxcgDWUQUTp/view?usp=sharing).

Education
======
* **Ph.D. in Computer Science**, University of Colorado Colorado Springs, *Expected 2026*
  * Advisors: Dr. Yanyan Zhuang and Dr. Gedare Bloom
  * Focus: IoT Malware Analysis, Binary Code Analysis, Machine Learning for Security
* **B.Sc. in Electrical and Electronic Engineering**, Chittagong University of Engineering and Technology

Honors & Awards
======
* O'NEIL Cyber Scholar, 2024
* Best Paper Award, IEEE CCWC 2024

Research Experience
======
* **Research Assistant**, University of Colorado Colorado Springs *(2021 – Present)*
  * Designed CIAO, a cross-architecture IoT malware classifier achieving 98.5% accuracy on 9,954 samples
  * Developed Cimalir, a cross-platform malware clustering tool using LLVM intermediate representation
  * Conducted firmware security analysis of 16 IoT devices, identifying encryption and secure boot gaps
  * Investigated operational similarity features across the malware development lifecycle

Teaching
======
* **Instructor**, CS-2080 Programming with UNIX, UCCS *(Fall 2025)*
* **Teaching/Research Assistant**, UCCS Computer Science Department

Skills
======
* **Reverse Engineering & Binary Analysis**: Ghidra, IDA Pro, Radare2, angr, LLVM IR, BinDiff
* **Programming**: Python, C/C++, Assembly (ARM, MIPS, x86, PowerPC)
* **Machine Learning**: PyTorch, scikit-learn, NetworkX, graph neural networks
* **Security Tools**: YARA, VirusTotal, Cuckoo Sandbox, Firmware Analysis Toolkit
* **Infrastructure**: Linux, Docker, Git, GitHub Actions

Publications
======
  <ul>{% for post in site.publications reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>

Service & Leadership
======
* Co-founder, UCCS Open Source Club
* Reviewer for IEEE and ACM conferences
