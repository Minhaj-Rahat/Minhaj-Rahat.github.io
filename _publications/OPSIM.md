---
title: "Poster: Operational Similarity in IoT Malware Development Life Cycle"
collection: publications
permalink: /publication/OPSIM
excerpt: 'This paper presents operational features in IoT malware extracted from  a large corpus of IoT malware that emerge from the shared development and logistics in the malware development life cycle (MDLC)'
date: 2025-05-06
venue: '23rd ACM Conference on Embedded Networked Sensor Systems (SenSys)'
paperurl: 'https://dl.acm.org/doi/10.1145/3715014.3724036'
citation: '@inbook{10.1145/3715014.3724036,
author = {Rahat, Minhajul Alam and Banerjee, Vijay and Bloom, Gedare and Zhuang, Yanyan},
title = {Poster Abstract: Operational Similarity in IoT Malware Development Life Cycle},
year = {2025},
isbn = {9798400714795},
publisher = {Association for Computing Machinery},
address = {New York, NY, USA},
url = {https://doi.org/10.1145/3715014.3724036},
abstract = {Due to hardware limitations and low cost, manufacturers often overlook security in widely deployed IoT devices, making them susceptible to attacks. Malware is one such attack that can cause extensive damage. In this work, we analyze a large corpus of IoT malware to extract features that emerge from the shared development and logistics in the malware development life cycle (MDLC), which we name operational features. We use these features to formulate a novel operational similarity concept for malware families and variants that complements existing code and behavior analysis.},
booktitle = {Proceedings of the 23rd ACM Conference on Embedded Networked Sensor Systems},
pages = {618–619},
numpages = {2}
}
'
---

The widespread adoption of Internet-of-Things (IoT) devices has significantly increased the interconnectivity of everyday activities. Malware on IoT devices directly jeopardize the security and privacy of their users, and device heterogeneity poses significant challenges in malware analysis. This paper presents Cimalir, which uses an automated approach for clustering malware that leverages an intermediate representation (IR) of low-level assembly code across different architectures. Cimalir uses a staged analysis approach that first filters binary images using set-based similarity of function attributes before applying call graph analysis. Additionally, Cimalir employs a string based technique to distinguish different malware families that exhibit a substantial level of shared code use. Cimalir has a low runtime complexity as evidenced by a linear relationship between execution time and the number of functions in the binary images under analysis. Experimental results show that Cimalir yields improved clustering results compared to the state-of-the-art binary analysis tool, BinDiff, when applied to malware binaries compiled for different instruction sets. Cimalir results in a DBCV score of 0.75 and Silhouette Score of 0.812 when clustering malware families from the CUBE-MALIOT-2021 dataset. In comparison, BinDiff achieves scores of 0.54 and 0.616, respectively. This indicates Cimalir’s superior performance compared to BinDiff. 