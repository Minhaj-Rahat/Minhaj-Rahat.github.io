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

The "evil maid" attack is a well-known concept in security: an attacker with physical access to an unattended device tampers with it in some undetectable way, then later accesses the device or the data on it. This is plausible for IoT devices — they sit in shared apartment buildings, hotels, schools, and offices. An attacker could pose as a technician, use insider privileges, or even buy used devices online.

Two attacks matter in this threat model: **private data theft** (extracting sensitive data from the device's storage) and **firmware modification** (replacing the firmware so the attacker gains persistent control). Defending against these requires two protection mechanisms:

1. **Encryption of data at rest** — If the flash storage is encrypted, dumping it yields nothing readable.
2. **Secure boot** — If the device verifies firmware signatures at each boot stage, modified firmware won't execute.

These two mechanisms are interdependent: if secure boot is broken, an attacker can flash modified firmware that extracts the encryption keys, defeating encryption too.

## The devices we tested

We analyzed 16 consumer IoT devices spanning smart speakers, cameras, hubs, thermostats, locks, plugs, and sensors. The lineup included well-known brands: Amazon Echo Show 5 and Echo Smart Speaker, Samsung SmartThings Hub V2 and V3, Ring Chime and Ring Bridge, Amcrest ProHD Shield camera, Honeywell Home thermostat, Philips Hue Bridge, Victure camera, Wyze Lock Gateway, Orbit b-Hyve sprinkler controller, Gosund WiFi Smart Plug, Govee Thermometer, iClever Air Quality Monitor, and ReFoss garage door opener.

Architectures included ARM, MIPS, and Espressif's Xtensa. Operating systems ranged from Linux to FreeRTOS, TI-RTOS, and bare metal.

## How we extracted the firmware

Getting the data off the storage chip was itself an exercise in how poorly some devices are protected. We used whatever method was simplest for each device:

- **Espressif devices** (Orbit, Wyze, Gosund, Govee, iClever): Espressif's own open-source `esptool.py` reads flash directly over UART. No chip removal needed.
- **Chip removal + reader**: For most non-Espressif devices (Amazon Echo, SmartThings, Amcrest, Honeywell, Ring Chime, Victure), we de-soldered the flash chip, connected probes to its pads, and used a Raspberry Pi with Flashrom over SPI to dump the contents.
- **Bootloader dump**: The Philips Hue Bridge had an accessible U-Boot command prompt over UART. We scripted a page-by-page NAND dump — no physical chip removal required.
- **One failure**: The ReFoss garage door opener used only on-chip flash (no external memory chip to remove), and we couldn't extract its storage through the SoC bootloader.

## What we found

### Data-at-rest encryption

- **10 out of 16 devices** had crypto-capable SoC/MCU hardware.
- **Only 2 actually encrypted their storage** — the Samsung SmartThings Hub V2 and V3, which used eFuse-stored keys as a root of trust and LUKS container encryption.
- The remaining 8 crypto-capable devices simply didn't use their hardware's encryption features.
- All 6 devices without crypto hardware were obviously unencrypted.
- **13 of 16 devices (81.3%)** were vulnerable to sensitive data compromise.

### Secure boot

- **9 out of 16 devices** had SoC/MCU hardware capable of secure boot.
- **Only 2 properly implemented it** (SmartThings V2 and V3, using U-Boot verified boot with eFuse-based chain of trust).
- **4 devices had capable hardware but didn't implement secure boot correctly.** For example, the Amcrest camera "verified" its kernel image with a checksum — which only checks for data corruption, not tampering. The attacker can recompute the checksum after modification.
- For Espressif devices, we verified the absence of secure boot by checking for a 32-byte secure digest at flash address 0x0. On both the Orbit b-Hyve and Wyze Lock Gateway, the value was `0xFFFFFFFF FFFFFFFF` — secure boot was never enabled.
- **11 of 16 devices (68.8%)** were vulnerable to firmware modification attacks.

## The case studies that alarmed us

**Ring Chime — HTTPS downgrade to 500+ firmware images.** We modified the Ring Chime's firmware to disable HTTPS, then booted the device with the modified firmware (it had no secure boot). The downgraded device connected to Ring's update server over plaintext HTTP, which let us identify the server and retrieve over 500 firmware images across various Ring ecosystem device types.

**Philips Hue Bridge — encryption key in plaintext storage.** We found the symmetric encryption key for firmware update packages sitting in the Philips Hue Bridge's persistent storage. We verified the key by successfully decrypting a firmware package.

**Used Amazon Echo Show 5 — previous owner's WiFi credentials.** A used Echo Show 5 we purchased online still contained the previous owner's network credentials in unencrypted storage. A factory reset didn't wipe them due to flash wear-leveling characteristics.

**Samsung SmartThings — the one that got it right.** The SmartThings hubs used SoC OTP eFuses to establish a root of trust and encrypted their data partitions with securely stored keys. This was the only manufacturer (across two devices) that implemented both encryption and secure boot in a way that actually prevented our attacks.

## The takeaway

Only one manufacturer out of 16 — Samsung with the SmartThings hubs — implemented both data-at-rest encryption and proper secure boot. The gap between hardware capability and actual implementation was the most striking finding: most devices *have the silicon* for encryption and secure boot, but the manufacturers never turned it on.

These aren't exotic protections. Encrypting data at rest and verifying firmware signatures are solved problems at the hardware level. The challenge is a manufacturing culture where security features are available but not enabled, likely because they add development complexity and testing burden without visible consumer-facing benefit.

The paper is available on [IEEE Xplore](https://ieeexplore.ieee.org/abstract/document/10427780).

---

*This work was a collaboration with Austen Knapp, Emmanuel Wamuo, Santiago Torres-Arias (Purdue), Gedare Bloom, and Yanyan Zhuang. If you're working on IoT firmware security or device hardening, I'd be glad to discuss our methodology — find me on [LinkedIn](https://linkedin.com/in/minhajul-alam-rahat-1aa157157) or email me at mrahat@uccs.edu.*
