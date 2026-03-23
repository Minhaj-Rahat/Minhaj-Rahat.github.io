---
title: "CIAO: Cross-architecture IoT Malware Family Classification with Code Reuse"
collection: publications
permalink: /publication/CIAO
excerpt: 'CIAO uses architecture-agnostic ESIL representations and graph representation learning (GIN) on Function Call Graphs to classify IoT malware families even under heavy code reuse, achieving 98.50% accuracy on 9,954 samples.'
venue: 'IEEE Consumer Communications & Networking Conference (CCNC)'
date: 2025-10-20
paperurl: 'https://ieeexplore.ieee.org/abstract/document/11366435/'
citation: '@inproceedings{rahat2026ciao,
  author={Rahat, Minhajul Alam and Banerjee, Vijay and Bloom, Gedare and Zhuang, Yanyan},
  booktitle={2026 IEEE Consumer Communications & Networking Conference (CCNC)},
  title={CIAO: Cross-architecture IoT Malware Family Classification with Code Reuse},
  year={2026},
  pages={1-6}
}'
---

**[PDF](/files/CIAO.pdf)** | [Source Code](https://github.com/Minhaj-Rahat/CIAO) | [Blog Post](/posts/2025/10/ciao-cross-architecture-iot-malware/)

## Abstract

Classifying malware variants into their respective families is a key challenge in malware analysis. This task is especially difficult with IoT malware because of code reuse and a variety of instruction set architectures (ISAs). Adversaries often reuse code across malware families through the same library functions, leaked source code, or known exploits. This practice makes it difficult for traditional Machine Learning (ML) classifiers to be reliable for malware classification. Further, IoT malware targets diverse instruction set architectures (ISAs), where differences in opcodes and registers create variations that hinder effective malware analysis. We present CIAO, a tool that uses an architecture-agnostic instruction representation and graph representation learning that captures family-specific logic even under heavy code reuse. Our evaluation of CIAO on a dataset of 9,954 IoT malware samples achieves an accuracy of 98.50%, a macro precision of 99.01%, and a macro recall of 98.94% in malware classification under code reuse, which outperforms existing techniques. Furthermore, in cross-architecture classification, where samples from one architecture are underrepresented, CIAO outperforms a model that uses architecture-specific instruction representation.
