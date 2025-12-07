---
title: "CIAO: Cross-architecture IoT Malware Family Classifiation with Code Reuse"
collection: publications
permalink: /publication/CIAO
excerpt: 'This paper presents Ciao, which applies graph representation learning on Function Call Graphs (FCGs) to model function interaction and separate reused code from unique functionalities. It introduces a novel malware family classifier that learns archtecture-independent binary representation to enable cross-architecture generalization in malware classification.'
venue: 'IEEE Consumer Communications & Networking Conference (CCNC)'
date: 2025-10-20

---

Classifying malware variants into their respective families is a key challenge in malware analysis. This task is especially difficult with IoT malware because of code reuse and a variety of instruction set architectures (ISAs). Adversaries often reuse code across malware families through the same library functions, leaked source code, or known exploits. This practice makes it difficult for traditional Machine Learning (ML) classifiers to be reliable for malware classification. Further, IoT malware targets diverse instruction set architectures (ISAs), where differences in opcodes and registers create variations that hinder effective malware analysis. We present CIAO, a tool that uses an architecture-agnostic instruction representation and graph representation learning that captures family-specific logic even under heavy code reuse. Our evaluation of CIAO on a dataset of 9,954 IoT malware samples achieves an accuracy of 98.50%, a macro precision of 99.01%, and a macro recall of 98.94% in malware classification under code reuse, which outperforms existing techniques. Furthermore, in cross-architecture classification, where samples from one architecture are underrepresented, CIAO outperforms a model that uses architecture-specific instruction representation.
