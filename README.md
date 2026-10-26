# Attempt 1: Omarchy

> Written from memory, with a few phone photos. Details may be imperfect.

## Summary

The installation and dual-boot process went smoothly, mostly because I have two
drives (C: and D:). I realised I could put Omarchy on the 1 TB SSD, which had no
Windows on it. That reduced the risk of accidentally wiping my Windows install.

However, a number of problems appeared after the installation, caused by errors
on my side and by not taking the process slowly.

## Preparation

- **Restore point:** I created a restore point for C:. It wouldn't help if I
  wiped the whole drive, but it could revert any damage on the Windows side.

<img width="400" alt="restore point" src="https://github.com/user-attachments/assets/c013645f-1287-49c5-abda-f241bba94f28" />


- **Drive layout:** I checked Disk Management. At first I planned to shrink the
  Windows drive and partition it, until I realised I could use the other drive
  instead.

<img width='400' alt="Disk Management before install" src="https://github.com/user-attachments/assets/9d22e606-0127-489d-9734-97ab5ec08171" />


- **BitLocker:** I turned off BitLocker on the drive Omarchy was going on, so
  Windows wouldn't interfere with the installation. I didn't have my recovery key
  on hand, but I could view it from another device through my Microsoft account.

<img width="400" alt="Bitlocker decrypting" src="https://github.com/user-attachments/assets/24d199f1-742a-4663-b498-81424fdbd2a8" />

<img width="400" alt="bitlocker decrypted" src="https://github.com/user-attachments/assets/efabba52-00f8-4c4e-871e-d4e45fdaf163" />

## Creating the USB

I used Rufus, a free, open-source tool for creating bootable USB drives, with the
Omarchy ISO on my own pendrive. I used the **GPT** partition scheme and formatted
the pendrive as **NTFS**.

<img width="400" alt="Rufus" src="https://github.com/user-attachments/assets/49630c1a-105c-4509-aaec-777513100e76" />


## BIOS changes

I turned Secure Boot off, because the installation guide I followed recommended
it, saying that leaving it on can cause issues with dual-boot setups. 

> **Warning:** Changing Secure Boot brought up a BitLocker warning. Back up your
> recovery key before changing it.

<img width="400" alt="Secure Boot" src="https://github.com/user-attachments/assets/d26082af-073f-430d-872d-6070fe7b35de" />

<img width="400" alt="image" src="https://github.com/user-attachments/assets/db57094a-4522-4284-9a7a-9106cf446ea4" />


## Installation

My C: drive (500 GB, about 475 GB usable) is the original drive and holds
Windows. The D: drive (1 TB) was installed later and was mostly empty. It only
had Steam and my Obsidian notes, and everything on it was easy to recover or
already backed up, so I had no problem wiping it and using it as Omarchy's boot
storage. When I discovered Omarchy could go on that drive without touching
Windows, I was ecstatic.

<img width="400" alt="D: content" src="https://github.com/user-attachments/assets/2df89640-e80e-4889-9310-d5942332ecdc" />


Once Rufus finished and decryption was complete, I restarted the laptop, pressed
**F12** to open the boot menu, and booted from the USB. The rest was
straightforward:

1. Chose the keyboard layout
2. Entered my username
3. Entered the hostname
4. Chose a time zone
5. **Chose the drive to install Omarchy on** (the most important step)
6. Chose the install type (full disk or free space). I chose **full disk**,
   since I had nothing to lose on that drive.

<img width="400" alt="image" src="https://github.com/user-attachments/assets/56f74437-3531-46b7-aa75-18af3e086a57" />

<img width="400" alt="Keyboard" src="https://github.com/user-attachments/assets/fa28e527-22b9-47af-b566-b44e46d44b01" />

<img width="400" alt="Username" src="https://github.com/user-attachments/assets/1a8e21ab-49c0-4155-ac6c-c7d8b614b168" />

<img width="400" alt="Hostname" src="https://github.com/user-attachments/assets/c4432812-632e-4d06-b083-f25b40b265f8" />

<img width="400" alt="timezone" src="https://github.com/user-attachments/assets/7e3edb94-4efc-4be7-bfce-d2c58ecc394a" />

<img width="400" alt="drive selection" src="https://github.com/user-attachments/assets/625a7299-65dc-4abb-a01f-df2061a8a09c" />

<img width="400" alt="Installation type" src="https://github.com/user-attachments/assets/b0ba133f-8003-42db-9058-49ce2ee81c5c" />


Omarchy installed successfully.

<img width="400" alt="Installation Complete" src="https://github.com/user-attachments/assets/98fe4d57-bd42-4e18-8a99-541d35459fa8" />


## Bootloader

Because Omarchy went on the 1 TB drive and Windows stayed on the 500 GB drive,
the two systems kept separate boot entries. I chose between them using
[the F12 boot menu]

<img width="400" alt="Windows" src="https://github.com/user-attachments/assets/09d115a4-fa75-4a4b-ac60-dcab96f487ea" />

<img width="400" alt="Omarchy" src="https://github.com/user-attachments/assets/035f89e1-a939-4ae8-8e57-56c9fbaa955c" />

## Problems and fixes

Several problems appeared after the install, on the Omarchy side, the Windows
side, and the hardware. Each one is documented with its cause and fix in
[attempt-1-problems.md](https://github.com/Sho-suj/Dual-booting-in-my-system/blob/main/Attempt-1-Omarchy/Attempt-1-problems).

## Final result

Omarchy installed successfully, and I could switch to Windows whenever I needed
to. I used the Linux terminal regularly to get comfortable with it.
