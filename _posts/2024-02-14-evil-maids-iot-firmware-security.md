---
title: 'We Checked 16 Smart Home Devices for Firmware Security. Most Failed. (Best Paper @ IEEE CCWC 2024)'
date: 2024-02-14
permalink: /posts/2024/02/evil-maids-iot-firmware-security/
tags:
  - IoT Security
  - Firmware Analysis
  - Embedded Systems
  - Research
---

Here's a question that should concern anyone with smart devices at home: if someone had physical access to your smart thermostat, security camera, or smart speaker for five minutes, could they extract your WiFi credentials, authentication tokens, or other sensitive data from the device's storage?

We tested 16 real consumer IoT devices to find out. The results were not encouraging.

Our paper **Should Smart Homes Be Afraid of Evil Maids? Identifying Vulnerabilities in IoT Device Firmware** was published at IEEE CCWC 2024, where it received the **Best Paper Award**.

## The evil maid threat model

The "evil maid" attack is a well-known concept in security: an attacker with brief physical access to a device tampers with it while the owner is away. For laptops, this might mean installing a keylogger on an unencrypted disk. For IoT devices, the attack surface is even wider — many of these devices were never designed to resist physical tampering.

We wanted to answer two specific questions about each device:

1. **Is data encrypted at rest?** If someone dumps the flash storage, can they read sensitive data in plaintext?
2. **Is secure boot implemented?** Can an attacker modify the firmware and have the device boot the tampered version?

## What we found

We manually analyzed the firmware of 16 IoT devices across different categories — smart speakers, cameras, thermostats, and other consumer products. The findings:

- **Only 2 out of 16 devices** encrypted data at rest, despite **10 having crypto-capable hardware**. The hardware supported encryption, but the manufacturers simply didn't turn it on.
- **9 devices had secure boot-capable hardware**, but **4 of those didn't properly implement it**. Having the silicon isn't enough — you have to actually use it correctly.
- **13 out of 16 devices** were vulnerable to sensitive data extraction.
- **11 out of 16 devices** were vulnerable to firmware modification attacks.

The gap between hardware capability and actual implementation was the most striking finding. These aren't cheap knockoff devices with no security silicon — they have the hardware, the manufacturers just didn't use it.

## How we analyzed the firmware

Our analysis methodology combined several techniques:

**Flash dump and extraction.** We physically extracted flash memory contents from each device using standard tools (bus pirate, logic analyzers, direct chip reading). For devices with eMMC storage, we used appropriate interfaces to access the raw data.

**Filesystem analysis.** We examined the extracted data for plaintext credentials, tokens, configuration files, and other sensitive information. We also checked for standard filesystem encryption indicators.

**Secure boot verification.** We examined the boot chain of each device to determine whether firmware signatures were verified at each stage. A device with secure boot hardware but no signature checks in the bootloader is effectively unprotected.

We proposed a systematic method for assessing both data-at-rest encryption and secure boot status that other researchers and analysts can apply to new devices.

## Why this matters beyond the lab

This isn't just an academic concern. IoT devices are deployed in homes, hospitals, factories, and critical infrastructure. A compromised smart home device can become a pivot point into the home network. A tampered industrial sensor can feed false data to control systems. As the installed base of IoT devices grows into the tens of billions, firmware security becomes a supply chain problem.

The good news: the fixes are known. Encrypting data at rest and implementing secure boot are solved problems at the hardware level — the silicon already supports it in most cases. The challenge is getting manufacturers to actually enable these protections in their firmware.

The paper is available on [IEEE Xplore](https://ieeexplore.ieee.org/abstract/document/10427780).

---

*This work was a collaboration with Austen Knapp, Emmanuel Wamuo, Santiago Torres-Arias, Gedare Bloom, and Yanyan Zhuang. If you're working on IoT firmware security or device hardening, I'd be glad to discuss our methodology and findings.*
