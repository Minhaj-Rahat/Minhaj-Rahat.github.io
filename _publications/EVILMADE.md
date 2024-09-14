---
title: "Should Smart Homes Be Afraid of Evil Maids? : Identifying Vulnerabilities in IoT Device Firmware"
collection: publications
permalink: /publication/EVILMADE
excerpt: 'This paper aims to evaluate the current state of IoT device firmware security and assess the effectiveness of existing methods in safeguarding sensitive data. We conducted a manual analysis of 16 IoT devices, revealing a range of firmware management techniques, each varying in their effectiveness against an evil maid attack scenario. '
date: 2024-02-13
venue: 'IEEE 14th Annual Computing and Communication Workshop and Conference (CCWC)'
paperurl: 'https://ieeexplore.ieee.org/abstract/document/10427780'
citation: '@INPROCEEDINGS{10427780,

  author={Knapp, Austen and Wamuo, Emmanuel and Rahat, Minhajul Alam and Torres-Arias, Santiago and Bloom, Gedare and Zhuang, Yanyan},

  booktitle={2024 IEEE 14th Annual Computing and Communication Workshop and Conference (CCWC)}, 

  title={Should Smart Homes Be Afraid of Evil Maids? : Identifying Vulnerabilities in IoT Device Firmware}, 

  year={2024},

  volume={},

  number={},

  pages={0467-0473},

  keywords={Performance evaluation;Data protection;Smart homes;Hardware;Encryption;Internet of Things;Microprogramming;Firmware Analysis;Data Encryption;Secure Boot},

  doi={10.1109/CCWC60891.2024.10427780}}
'
---

The Internet of Things (IoT) revolution has transformed everyday consumer objects into interconnected, intelligent devices. Due to historically weak security designs, these devices are susceptible to compromises with far-reaching consequences. This paper aims to evaluate the current state of IoT device firmware security and assess the effectiveness of existing methods in safeguarding sensitive data. We conducted a manual analysis of 16 IoT devices, revealing a range of firmware management techniques, each varying in their effectiveness against an evil maid attack scenario. Out of the 16 devices, only 2 showed evidence of encrypting data at rest, despite 10 having crypto-enabled hardware. Additionally, 9 out of 16 devices possessed secure boot-enabled hardware, but 4 of them did not properly utilize or implement secure boot. Consequently, 13 devices were identified as vulnerable to sensitive data compromise, and 11 were at risk of firmware modification attacks. To address these critical security gaps, this study proposes a method for analyzing data-at-rest encryption and secure boot status in IoT devices. In this paper we shed light on the prevailing security shortcomings and provide practical analysis techniques to foster improved IoT device security and user data protection. 