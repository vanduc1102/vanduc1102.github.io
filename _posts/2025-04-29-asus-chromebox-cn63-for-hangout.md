---
layout: post
title: Upgrading Asus Chromebox CN62 GUADO for Meet version
date: 2025-04-29 06:14:27 +0700
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

- I will try another ways, if I can get into the `Dev Mode`, I will install new firmware from [MrChromebox](https://docs.mrchromebox.tech/), then Windows 11

## Update 2025-05-12

- `Ctrl+D` works on Logitech keyboard, I failed many times with a mechanic keyboard.
- Before enter DevMode, you are better to PowerWash the Chrombox.
- Upgraded to 16GB RAM DDR3 PCL (1.35v) , SSD 256GB
- I've installed dual boot for Windows 10 and Fedora 42
- To install Windows, You will have to create bootale USB from Windows, Download the Windows images from official page, and use Rufus to create the bootable USB. I failed many times with bootable USB created from Linux and Mac.
- I installed Windows 11 successfully on original 4GB RAM, but when I replaced to 16GB RAM, the installation keep failing. I have to install Windows 10 ( less picky).
- The Hangout Remote works on Windows and Linux, You can repair by holding both `Menu + Mute` buttons at the same time for 4-6 seconds.
- Huge thanks to MrChromebox, you are helping to save environments.

## References

- [MrChromebox Documentation](https://docs.mrchromebox.tech/)
- [Reddit - CN62 Asus Chrombox out of date](https://www.reddit.com/r/chromeos/comments/tw4p6f/cn62_asus_chrombox_out_of_date/)
- [Stack Overflow - How to enter developer mode on Asus Chromebox](https://stackoverflow.com/questions/25980648/how-do-you-enter-developer-mode-on-the-asus-chromebox)
- [Chromium OS Developer Information](https://www.chromium.org/chromium-os/developer-library/reference/development/developer-information-for-chrome-os-devices/generic/)
