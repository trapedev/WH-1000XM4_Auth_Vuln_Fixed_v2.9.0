# Sony WH-1000XM4: Improper Authentication Vulnerability Fixed in v2.9.0

## 0. Executive Summary

- This report details an authentication vulnerability identified in Sony's WH-1000XM4 product.

- CVSS:3.0/AV:A/AC:L/PR:N/UI:N/S:U/C:L/I:H/A:H

- We responsibly disclosed our findings to the Sony in Jan. 2025.

- The vulnerability is the same type of vulnerability reported in [this](https://github.com/trapedev/WH-1000XM5_Auth_Vuln_Fixed_v2.4.1) repository.

## 1. Vulnerability Type

[CWE-287](https://cwe.mitre.org/data/definitions/287.html)

## 2. Vulnerability Details

A malicious third party (hereinafter referred to as the attacker) can impersonate a device that has been paired with the WH-1000XM4, allowing the attacker's device to connect to the WH-1000XM4 even when it is **not** in pairing mode and **without** requiring any operation from the WH-1000XM4 user.

Upon examining the Bluetooth packets, it appears that there is a flaw in the authentication process during re-connection of the WH-1000XM4.

## 3. Affected Specification

[WH-1000XM4](https://electronics.sony.com/audio/headphones/headband/p/wh1000xm4-b) (Firmware version: v2.7.1 and earlier)

## 4. PoC

This section presents the devices and setup required for the PoC, as well as the steps to reproduce this vulnerability.

### 4.1. Notation

- Alice: Victim, Master
- Bob: Victim, Slave
- Mallory: Attacker

### 4.2. PoC Devices

**Alice's Specification**

| Manufacturer | Model            | Operation System | Driver                         | Bluetooth Version |
| ------------ | ---------------- | ---------------- | ------------------------------ | ----------------- |
| Microsoft    | Surface Laptop 4 | Windows 11 Home  | Intel(R) Wireless Bluetooth(R) | 5.1               |

**Bob's Specification**

| Manufacturer     | Model      | Bluetooth Version | Firmware Version |
| ---------------- | ---------- | ----------------- | ---------------- |
| Sony Corporation | WH-1000XM4 | 5.0               | 2.5.3 and 2.7.1  |

**Mallory's Specification**

| Model                  | Operation System | System | Debian version | Kernel version | BlueZ version | Bluetooth Manufacturer | Bluetooth Version |
| ---------------------- | ---------------- | ------ | -------------- | -------------- | ------------- | ---------------------- | ----------------- |
| Raspberry Pi 4 Model B | Raspberry Pi OS  | 32bit  | 11 bullseye    | 6.1            | 5.55          | Cypress Semiconductor  | 5.0               |

### 4.3. PoC Setup

Pair the WH-1000XM4 and Surface Laptop 4 in advance.

### 4.4. PoC Procedure

1. Spoof the Bluetooth address of the Raspberry Pi and the Bluetooth name of its adapter to match those of the Surface Laptop 4.
2. Set the Bluetooth adapter state of the Raspberry Pi to Discoverable.
3. Leave the WH-1000XM4 idle for a while or manually turn off its power.
4. Turn off the Surface Laptop 4.
5. Turn on the WH-1000XM4.

If you follow these steps in order, the WH-1000XM4 will pair and connect to the Raspberry Pi despite **not** being in pairing mode. Please note that the Raspberry Pi has **never previously paired** with the WH-1000XM4.

The following image is the sequence diagram of this vulnerability.

<img src="https://github.com/user-attachments/assets/ea2ea7a7-f8fb-4578-9b61-eec36e58c1b0" height="75%"/>

### 4.5. PoC Packets

- [v2.7.1](./packets/WH-1000XM4-v2.7.1.pcapng)

## 5. Responsible Disclosure

On January 12, 2025, we reported this vulnerability to Sony via HackerOne.

On April 8, 2025, a patch for this vulnerability was released as v2.9.0.

## 6. References

- https://www.sony.jp/headphone/update/ (Access: 8 April, 2025)
- https://www.sony.com/electronics/support/wireless-headphones-bluetooth-headphones/wh-1000xm4/software/00261613 (Access: 8 April, 2025)
- https://www.sony-asia.com/electronics/support/wireless-headphones-bluetooth-headphones/wh-1000xm4/software/00261612 (Access: 8 April, 2025)

## 7. Acknowledgements from Sony

- In Japanese

![japanese](https://github.com/user-attachments/assets/31fa4783-d2ab-4487-aaf0-f0e15648e8d2)

- In English

![english](https://github.com/user-attachments/assets/bc51b590-7a47-42da-abbf-d79d46ddb022)

## 8. Notes

Discoverer: Keiichiro KIMURA

Collaborators: Prof. Hiroki KUZUNO, Prof. Yoshiaki SHIRAISHI, and Prof. Masakatu MORII (Kobe University)

We express our deep gratitude for Sony's prompt response.
