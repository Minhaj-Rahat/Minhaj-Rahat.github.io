---
title: "Cimalir: Cross-Platform IoT Malware Clustering using Intermediate Representation"
collection: publications
permalink: /publication/CIMALIR
excerpt: 'This paper presents Cimalir, which uses an automated approach for clustering malware that leverages an intermediate representation (IR) of low-level assembly code across different architectures'
date: 2024-02-13
venue: 'IEEE 14th Annual Computing and Communication Workshop and Conference (CCWC)'
paperurl: 'https://ieeexplore.ieee.org/abstract/document/10427663'
citation: '@INPROCEEDINGS{10427663,

  author={Rahat, Minhajul Alam and Banerjee, Vijay and Bloom, Gedare and Zhuang, Yanyan},

  booktitle={2024 IEEE 14th Annual Computing and Communication Workshop and Conference (CCWC)}, 

  title={Cimalir: Cross-Platform IoT Malware Clustering using Intermediate Representation}, 

  year={2024},

  volume={},

  number={},

  pages={0460-0466},

  keywords={Privacy;Codes;Runtime;Instruction sets;Malware;Internet of Things;Security;IoT;malware;intermediate representation},

  doi={10.1109/CCWC60891.2024.10427663}}
'
---

The widespread adoption of Internet-of-Things (IoT) devices has significantly increased the interconnectivity of everyday activities. Malware on IoT devices directly jeopardize the security and privacy of their users, and device heterogeneity poses significant challenges in malware analysis. This paper presents Cimalir, which uses an automated approach for clustering malware that leverages an intermediate representation (IR) of low-level assembly code across different architectures. Cimalir uses a staged analysis approach that first filters binary images using set-based similarity of function attributes before applying call graph analysis. Additionally, Cimalir employs a string based technique to distinguish different malware families that exhibit a substantial level of shared code use. Cimalir has a low runtime complexity as evidenced by a linear relationship between execution time and the number of functions in the binary images under analysis. Experimental results show that Cimalir yields improved clustering results compared to the state-of-the-art binary analysis tool, BinDiff, when applied to malware binaries compiled for different instruction sets. Cimalir results in a DBCV score of 0.75 and Silhouette Score of 0.812 when clustering malware families from the CUBE-MALIOT-2021 dataset. In comparison, BinDiff achieves scores of 0.54 and 0.616, respectively. This indicates Cimalir’s superior performance compared to BinDiff. 