---
layout: post
title: Upgrading Asus Chromebox CN62 GUADO for Meet version
date: 2025-04-30 06:14:27 +0700
categories: [computer]
tags: [minpc, os]
---

I've got 2 Asus Chromeboxes CN62 GUADO for Meet (CFM)

They are both unenrolled, it was for my company meeting before.

Here the detail I get from the devices

```shell
Google Chrome: Version 91.0.4456.0 (Official Build) dev (64-bit)
Platform: 13870.0.0 (Official Build) dev-channel guado
Channel: dev-channel
Firmware Version: Google_Guado.6301.108.4
ARC Enabled: false
ARC:
Enterprise Enrolled: false
Developer Mode: false
```

## Hardware specification

ASUS Chromebox 2 (CN62) for meetings G015U

- USFF
- 1 x Core i7 5500U / 2.4 GHz
- RAM 4 GB
- SSD 16 GB
- HD Graphics 5500
- GigE
- WLAN: Bluetooth 4.0, 802.11b/g/n/ac
- Chrome OS
- monitor: none

## Things work

- Replaced the Storage of 16GB to 256GB , it is M.2 SATA SSD
- Upgraded the RAM from 4GB to 16GB, the RAM is DDR3L for Laptop 1.35V. Voltage is important, I bought 8GBx2 RAM , but it was 1.5V, they are not compatible.
- I've created an USB to recovery, 4GB USB is accepted.

## Things do not work

- Many tutorials say at the recovery screen, hit `Ctrl+D` to enter the `Dev Mode`, But it does not work for me. Tried so many times.

- I will try another ways, if I can get into the `Dev Mode`, I will install new firmware from https://docs.mrchromebox.tech/, then Windows 11

## References

- https://docs.mrchromebox.tech/
- https://www.reddit.com/r/chromeos/comments/tw4p6f/cn62_asus_chrombox_out_of_date/
- https://stackoverflow.com/questions/25980648/how-do-you-enter-developer-mode-on-the-asus-chromebox
- https://www.chromium.org/chromium-os/developer-library/reference/development/developer-information-for-chrome-os-devices/generic/
