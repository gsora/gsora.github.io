---
layout: post
title:  “Convert Arch Linux installation from MBR/BIOS to UEFI“
date:   2015-11-30 16:32
categories: linux arch archlinux pc mbr bios uefi
---

After literally years of notebook-only computing I finally built a nice desktop PC, and since I didn’t had enough money to afford an SSD drive right now, I’m borrowing it from my trusty ThinkPad X61s.

The Arch Linux installation I had there was already configured to fit my needs and I wanted to test the real flexibility this distro have to offer, so I converted it from a MBR/BIOS system to a UEFI one, here’s how I did it using only the Arch installation media.

# Backup.

Always make a backup before doing this kind of things.

# Analyzing the partition table

Start the computer from the Arch installation media in UEFI mode first.

The UEFI system relies on a special partition called *”EFI System Partition”*, abbreviated in `*ESP*`, where the OS bootloader and often its configuration files resides.

So, the first step is to make room on the disk for this partition.

My partition table was composed from:

    /dev/sda1: root filesystem
    /dev/sda2: swap partition
    
Since this new PC have enough RAM already, I simply deleted the swap and extended `sda1` enough to make a 200MB, `ef00` type `ESP` partition.

# Convert the partition table

To boot with UEFI, you need a `gpt` partition table and luckily a `MBR` partition table can become one easily.

Run `gdisk /dev/sdX`, the software will warn you that if you invoke the `w` command the partition table will be converted to `gpt`, and we want exactly that.

# Follow the white rabbit

The last thing to do is pretty much follow the [Beginner’s guide](https://wiki.archlinux.org/index.php/Beginners'_guide):
    
 - mount your root filesystem on `/mnt` but **do not** format it
 - create a `/mnt/boot/uefi` mountpoint
 - format your `ESP` partition with `mkfs.vfat /dev/sdX`
 - mount it on the directory created previously
 - rebuild your `fstab` file with `genfstab -U /mnt/ > /mnt/etc/fstab` to reflect the changes 
 - chroot in your root
 - build a `grub.cfg` file and then install the bootloader as the guide says.
 
Done, now your system will boot in UEFI mode!
