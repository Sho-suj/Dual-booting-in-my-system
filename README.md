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


# Attempt 2: Fedora

> Written from memory, cross-checked against my own notes. Details may be
> imperfect.

## Reclaiming the drive from Omarchy

Before I could do anything else, I had to deal with the 1 TB SSD still being
formatted as LUKS-encrypted Btrfs from Omarchy, which Windows can't read or
resize. I needed it back to plain, usable space first.

I opened Command Prompt as Administrator and used DiskPart:
1. _diskpart_
2. _list disk_
3. _select disk 0_
4. _detail disk_
5. _clean_
6. _convert gpt_
7. _create partition primary_
8. _format fs=ntfs quick_
9. _assign_

<img width="400" alt="Diskpart command line" src="https://github.com/user-attachments/assets/f59b690c-5776-4a4b-b0af-d961731bd4b3" />

- `list disk` showed Disk 0 as 953 GB (the 1 TB SSD that had Omarchy) and Disk
  1 as 476 GB (the Windows drive), which confirmed I had the right one
  selected before going any further.
- `detail disk` double-checked it.
- `clean` wiped the Omarchy partitions, leaving raw unallocated space.
- `convert gpt`, `create partition primary`, `format fs=ntfs quick`, and
  `assign` turned that space back into a normal NTFS partition Windows could
  use.

> **Warning:** `clean` wipes the entire selected disk. Since the 1 TB drive
> held only Omarchy at this point, this was safe for me, but always confirm
> the disk number with `list disk` and `detail disk` before running it. On a
> different machine, the numbers may not match mine, so never copy `select
> disk 0` blindly.

<img width="400" alt="Disk after command" src="https://github.com/user-attachments/assets/ab9635bd-78f0-429f-8408-b3104939cdba" />

## Preparation

- **Fast Startup:** Turned off Fast Startup in Windows, since it can leave
  the Windows filesystem in a partially hibernated state, which is unsafe for
  another OS to touch.
- **BitLocker:** Left BitLocker alone on both C: and Terra this time, rather
  than turning it off like I did in attempt 1.
- **Virtual Machine Practice:** Like omarchy I also practiced installing
  fedora in a virtual machine first and came across a loop problem where
  fedora after completion kept relooping and the installation process
  began again. More will be discussed in the problems section for it.

I downloaded the Fedora ISO and Fedora Media Writer at the same time.

<img width="400" alt="Fedora ISO and Fedora Media Writer downloads" src="https://github.com/user-attachments/assets/5cf4d9c2-6d83-4672-a478-dded0e3b7c57" />

While those were downloading, I partitioned Terra, giving Fedora 307,200 MB
(300 GB) of it. I did this to make Terra a shareable drive between Windows
and Fedora, rather than handing over the whole thing like I did with Omarchy.

<img width="400" alt="Shrinking Terra to free up space for Fedora" src="https://github.com/user-attachments/assets/e30a67ef-26ce-40dd-85ec-305fdd49c747" />

For the ISO, I chose the **KDE Plasma** version of Fedora, and downloaded
**Fedora Media Writer 5.3.2**. I could have reused Rufus for this, but I
wanted to try Fedora Media Writer instead, since I figured using Fedora's own
tool would help me get more familiar with its ecosystem from the start.

<img width="400" alt="KDE Plasma" src="https://github.com/user-attachments/assets/36425ee8-257c-4e47-acb8-08140bb68b25" />

In Fedora Media Writer, I selected the ISO I'd downloaded and wrote it to my
pendrive.

<img width="787" height="627" alt="Fedora Media Writer writing the ISO" src="https://github.com/user-attachments/assets/18941130-2bc0-4d1e-b7da-41b5d1619838" />

## Installation

I shut down my laptop fully, then tapped **F12** once for the boot menu. I
then booted from the USB, which started the installer for Fedora.

The installation process itself was very straightforward:

1. **Language and keyboard layout:** picked both at the start.

<img width="400" alt="Language selection" src="https://github.com/user-attachments/assets/6f2d4448-060a-4e5d-97bc-069ee975047f" />

2. **Destination:** selected **Terra** as the install target. I did this very
   carefully, since a simple mistake here could wipe the whole Windows side.

<img width="400" alt="Installation destination set to Terra" src="https://github.com/user-attachments/assets/60676991-21e9-4587-a181-bc83ef1f6a94" />

3. **Encryption passphrase and keyboard layout at boot:** set a passphrase
   for the new Fedora partition, and configured the keyboard layout
   separately for the boot/decrypt screen.

<img width="400" alt="Encryption passphrase prompt" src="https://github.com/user-attachments/assets/a8b8ec6a-c2ff-4028-9401-30c159d45bb4" />

After confirming everything, I waited for the installer to finish.

<img width="400" alt="Fedora installing" src="https://github.com/user-attachments/assets/8c58a0d7-5401-4ec9-8dbf-6412e2d8e887" />

Fedora installed successfully, sharing the 1 TB drive with Terra rather than
taking the whole disk like Omarchy had.

<img width="400" alt="Fedora installation complete" src="https://github.com/user-attachments/assets/60d07b9d-46cd-4f69-b80c-3844f303d20c" />

<img width="400" alt="Disk partition confirmed working" src="https://github.com/user-attachments/assets/8257d680-4220-4682-babb-815a0f863497" />

## Bootloader

Like Omarchy, Fedora was installed on the 1 TB drive with a separate boot 
entry in the UEFI firmware. I could switch between the two operating systems
using the F12 boot menu.

<img width="400" alt="Boot Menu" src="https://github.com/user-attachments/assets/908fe627-1174-40e8-84e7-7c8c5dd7c4ab" />


## Problems and fixes

There weren't any problems that arose in fedora, it was mostly smooth with no 
major complains from my side. The few problem I had were mostly related to 
beginner problems with the terminal which have been documented in the problems
section.

## Final result

Fedora installed successfully, and I could switch to Windows and use a shared
SSD to share files between fedora and Windows. After using it for a couple of 
months, I got more used to the terminal command and linux in general.
