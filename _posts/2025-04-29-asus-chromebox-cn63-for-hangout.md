---
layout: post
title: Upgrading Asus Chromebox CN62 GUADO for Meet version
date: 2025-04-29 06:14:27 +0700
categories: [computer]
tags: [minpc, os]
---

I acquired 2 Asus Chromebox CN62 GUADO for Meet (CFM) units.

They are both unenrolled devices previously used for company meetings.

Here are the system details from the devices:

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

## What Worked

- Replaced the 16GB storage with a 256GB M.2 SATA SSD.
- Upgraded the RAM from 4GB to 16GB using DDR3L laptop memory (1.35V). Voltage is critical: standard 1.5V DDR3 RAM is incompatible.
- Created a recovery USB (a 4GB drive is sufficient).

## What Did Not Work

- Many tutorials say to hit `Ctrl+D` at the recovery screen to enter Developer Mode, but it did not work for me initially despite many attempts.

- I will explore other methods; once in Developer Mode, I plan to install custom firmware from [MrChromebox](https://docs.mrchromebox.tech/) and then Windows 11.

## Update 2025-05-12

- `Ctrl+D` worked using a Logitech membrane keyboard; it repeatedly failed when using a mechanical keyboard.
- Before entering Developer Mode, it is best to Powerwash the Chromebox.
- Upgraded to 16GB RAM DDR3L (1.35V) and a 256GB SSD.
- Configured dual-booting with Windows 10 and Fedora 42.
- To install Windows, create the bootable USB from a Windows machine using Rufus with the official ISO image. Creating a bootable USB on macOS or Linux failed repeatedly.
- Windows 11 installed successfully with the original 4GB RAM, but when upgraded to 16GB RAM, the installation kept failing. I opted for Windows 10 instead, which is less picky.
- The Hangouts Remote works on both Windows and Linux. You can re-pair it by holding both `Menu + Mute` buttons simultaneously for 4–6 seconds.
- Huge thanks to MrChromebox for helping keep old hardware functional and out of landfills.

## Update 2025-05-16

- Replaced the broken CPU cooling fan. The fan is compatible with Intel NUC models (NUC5i7RYH, NUC6i3SYH, NUC6i3SYK, NUC6i5SYH, NUC6i5SYK, NUC7i3BNH); I ordered a replacement on shopee.vn.

## References

- [MrChromebox Documentation](https://docs.mrchromebox.tech/)
- [Reddit - CN62 Asus Chrombox out of date](https://www.reddit.com/r/chromeos/comments/tw4p6f/cn62_asus_chrombox_out_of_date/)
- [Stack Overflow - How to enter developer mode on Asus Chromebox](https://stackoverflow.com/questions/25980648/how-do-you-enter-developer-mode-on-the-asus-chromebox)
- [Chromium OS Developer Information](https://www.chromium.org/chromium-os/developer-library/reference/development/developer-information-for-chrome-os-devices/generic/)
