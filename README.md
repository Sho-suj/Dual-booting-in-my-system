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
<img width="1602" height="861" alt="image" src="https://github.com/user-attachments/assets/edb362ee-d2ca-4319-80ba-9f2efac9348d" />

---

# Attempt 3: Arch

> Well documented, with screenshots of everything done.

## Summary

Initially, this was going to be another dual-boot attempt, installing
Pop!_OS. But while checking Facebook Marketplace for cheap systems, I managed
to get a Lenovo ThinkPad X230 for £50.

## Preparation

- **Research:** I read through the Arch wiki briefly and watched videos on
  YouTube. Luckily, the Arch ISO comes with `archinstall`, a guided installer
  which makes the installation a lot simpler. I would still recommend reading
  the wiki, as installing Arch manually makes you more knowledgeable about
  Linux in general:
  https://wiki.archlinux.org/title/Installation_guide
- **Backups:** As it was a new system, I didn't have to back anything up or
  worry about wiping anything of value, as there was nothing of value on it.

## Downloading the ISO

I downloaded the ISO from an Arch mirror over https. Once the download
finishes, you should check its hash to make sure the file wasn't corrupted or
tampered with.

<img width="400" alt="ISO image download" src="https://github.com/user-attachments/assets/cc17dcc4-c687-42ba-a162-c45a1b38c8be" />

To generate the hash, I opened PowerShell and ran:
`Get-FileHash <ISO location> -Algorithm SHA256`
Then I compared the result to the matching line in `sha256sums.txt`, which sits
in the same folder as the ISO on the mirror.

<img width="400" alt="Checking the hash" src="https://github.com/user-attachments/assets/62745b94-3f99-4b90-b1c3-be66ca66be5f" />

> **Note:** a hash from the same mirror only proves the download wasn't
> corrupted. For a stronger check, compare it to the hash on
> archlinux.org/download or verify the `.sig` file with `gpg`.

## Creating the USB

Like Omarchy, I used Rufus to flash the ISO onto the USB. As I had uninstalled
Rufus, I downloaded it through winget, as it was much quicker than going to the
website.

<img width="400" alt="Rufus download through winget" src="https://github.com/user-attachments/assets/cd529cff-4b59-4f00-9dc6-6598e28bbc7a" />

I used the **GPT** partition scheme, **UEFI** as the target system and
**FAT32** as the file system, then waited for the flashing to finish.

> **Warning:** Rufus erases everything on the selected USB, so check the
> device name before pressing Start.

*Note: the Arch wiki lists various other ways to flash the USB, which I found
interesting, so definitely give it a read.*

<img width="400" alt="Rufus" src="https://github.com/user-attachments/assets/defd2bf6-b62d-4f62-afaf-154e0a601cae" />

<img width="400" alt="Rufus complete" src="https://github.com/user-attachments/assets/b024c981-992b-44f0-8161-72fabbd6e512" />

## Installation

Like the previous attempts, I booted from the USB by pressing **F12** at
startup to open the boot menu and picking the Generic Flash Disk.

<img width="400" alt="Boot menu" src="https://github.com/user-attachments/assets/2b2b4720-558e-402c-bba0-626adc9e17a2" />

1. **Boot loader menu:** chose **Arch Linux install medium**.

<img width="400" alt="boot loader menu" src="https://github.com/user-attachments/assets/1e8b2248-2fc5-4969-8927-705d0e171a69" />

2. **Terminal:** I was met with a black, terminal-like screen with just a text
   window.

<img width="400" alt="CLI" src="https://github.com/user-attachments/assets/aa636db9-9af1-4e94-b7d6-f7fbb8352bf9" />

3. **Internet check:** I checked my IP address with `ip a`, then pinged with
   `ping -c 5 8.8.8.8`. Here `-c 5` sends 5 pings and 8.8.8.8 is one of
   Google's public DNS servers. Pinging an address only proves the connection
   works, so I'd also try `ping -c 5 archlinux.org` to check DNS.

<img width="400" alt="image" src="https://github.com/user-attachments/assets/a9efb862-5626-4d79-88e4-5fb3d44513ab" />

4. **Wi-Fi:** I got no IP and no replies, so I had no connection. I ran
   `iwctl`, a tool for connecting to Wi-Fi, checked the device list, viewed
   the available networks, found mine and connected with the password.

<img width="400" alt="iwctl menu" src="https://github.com/user-attachments/assets/4f2e0e7d-4233-41aa-a4ca-cca084c7ae31" />

> **Note:** you can skip this with an Ethernet connection, which I didn't
> have.

5. **SSH for better screenshots:** To document the installation better, and
   not just take photos with another device, I set up SSH so I could take
   clearer screenshots from my main laptop. For this you need a root password,
   which you set with `passwd`. It's completely optional, but I did it for
   higher quality documentation.

<img width="400" alt="pwd root" src="https://github.com/user-attachments/assets/9340012b-2ed3-4524-82b2-dbad82dc6179" />

(screenshot taken from a YouTube video, not my actual screenshot, since I didn't have it)

> **Note:** both computers need to be on the same network. If you use a VPN on
> the other computer, turn it off or allow local network connections. Mine
> blocked the connection.

6. **Connecting over SSH:** From my main system, I opened PowerShell and
   connected with `ssh root@<ip address>`, then entered the root password I had
   set.

<img width="600" alt="SSH connected" src="https://github.com/user-attachments/assets/e45233ba-f1cb-4fc6-8e62-72f0182a48e0" />

7. **Starting archinstall:** Once connected, I typed `archinstall`, which starts
   the guided installer and makes the whole process a lot easier.

<img width="600" alt="archinstall language" src="https://github.com/user-attachments/assets/a259376a-20ee-4ea1-b426-add995993160" />

## Configuring archinstall

From here, most of the process is simple menu choices. I went through them in
this order:

1. **Language and keyboard layout:** picked both at the start.

<img width="600" alt="keyboard settings" src="https://github.com/user-attachments/assets/33965496-266d-450e-bbb3-59da53d08eee" />

2. **Mirrors and repositories:** selected my mirror region.

<img width="400" alt="mirrors and repository" src="https://github.com/user-attachments/assets/4e487277-0a88-43bf-9fe0-0790f92bddb2" />

3. **Disk configuration:** selected the disk to install on, which is normally
   your SSD and **not the USB**, then chose **ext4** as the file system. I left
   swap as the default.

> **Warning:** the installer wipes the disk you select. Check the device name
> and size carefully, so you don't pick the installer USB or a disk you want to
> keep.

<img width="600" alt="disk configuration" src="https://github.com/user-attachments/assets/daaec69f-42dc-4f44-a1f2-6b6af69e20a5" />

<img width="600" alt="disk selection for installation" src="https://github.com/user-attachments/assets/8db22a28-76e0-4281-8d35-8151d106e548" />

<img width="400" alt="main file system" src="https://github.com/user-attachments/assets/b65f6636-e0b1-484a-97c3-68501c9d41f0" />

<img width="400" alt="swap" src="https://github.com/user-attachments/assets/3d8ce047-1688-4004-9fda-9eaf37e81ed7" />

4. **Bootloader and kernels:** picked a bootloader and installed two kernels:
   `linux` (the main one) and `linux-lts` (a long-term support backup, in case
   an update breaks the main one).

<img width="400" alt="bootloader" src="https://github.com/user-attachments/assets/156a2ced-3a50-4b53-94dc-b16730c5fc9f" />

5. **Hostname:** set the hostname to `archie`.

<img width="400" alt="hostname" src="https://github.com/user-attachments/assets/1cd566e3-662c-47c2-a8ac-d65938544024" />

6. **Root password and user account:** set a root password and created my user
   account.

<img width="400" alt="root password, user account" src="https://github.com/user-attachments/assets/66dcfded-3bec-470a-8837-af2609326891" />

<img width="400" alt="useraccount" src="https://github.com/user-attachments/assets/2c662722-cb94-4f26-bf2e-69a5eb156f1a" />

<img width="400" alt="Screenshot 2026-11-20 013409" src="https://github.com/user-attachments/assets/b9aef533-f40a-4ded-9211-d83b6f2d9b5b" />

<img width="400" alt="Screenshot 2026-11-20 013431" src="https://github.com/user-attachments/assets/5b6f0a3a-88ae-412e-914b-f09b6b1df7c6" />

7. **Profile and desktop environment:** chose **Desktop** as the profile type,
   then picked **Xfce4** as the desktop environment. I didn't like it, so
   after the install I switched to **GNOME**. I also tried **Hyprland**, which
   I really liked, but this old laptop couldn't run it smoothly and it hurt
   performance, so I stayed on GNOME.

<img width="600" alt="Desktop envirnoment" src="https://github.com/user-attachments/assets/9170421d-c551-440f-a5cb-f9f0a2ac7aa8" />
<img width="400" alt="Screenshot 2026-11-20 013431" src="https://github.com/user-attachments/assets/5b6f0a3a-88ae-412e-914b-f09b6b1df7c6" />

8. **Graphics driver and greeter:** picked the **Intel (open-source)** graphics
   driver, since the X230 has Intel graphics, and left the greeter as default.

<img width="600" alt="graphics driver" src="https://github.com/user-attachments/assets/9d1ad086-944e-458d-9f86-1134ceaee3b2" />

<img width="400" alt="Profile overview" src="https://github.com/user-attachments/assets/744ee5e0-a750-4713-819b-c95c53012d99" />

9. **Applications, network and extra packages:** left applications and network
   at their defaults. The only additional package I added was `firefox`.

<img width="400" alt="additional package" src="https://github.com/user-attachments/assets/7f3e21f9-8159-4d68-97ae-d06be6d1f803" />

10. **Time zone:** set my time zone.

Below is a screenshot of the full configuration before starting the
installation.

<img width="600" alt="overview" src="https://github.com/user-attachments/assets/6fdac084-1d01-4ad8-9b72-873f9434e87a" />

## Running the install

After checking the overview, I pressed **Install** and waited for it to finish.

<img width="600" alt="installing" src="https://github.com/user-attachments/assets/53a98960-0249-4452-b8d0-76e3006b0904" />

Once it was done, I chose **Reboot system**, and Arch was ready to use.

<img width="600" alt="reboot" src="https://github.com/user-attachments/assets/bf233863-6947-417e-a127-dce3342b5543" />

## Problem and Fixes

During the installation one major problem arose, which was caused due to bad mirror which will
be touched up more in the probelms with attempt 3 section

## Final Result 

Arch was installed successfully, and I'm currently trying out different
desktop environments to see which one I like most. Arch made the whole
system feel really smooth to run, though it still doesn't compare to a
modern system's speed. Still, comparing a decade-old laptop to something
modern and seeing it keep up as well as it does is genuinely surprising.

The ThinkPad lives up to its reputation: built to last, and very
upgradable. I've seen people add more storage, swap in better displays, and
modify ThinkPads in all kinds of ways, which could make for some good
projects of my own in the future.

Overall, I really enjoyed everything that happened: the good, the bad, and
the workarounds. The whole learning experience was fun.
