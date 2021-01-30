# Installation

## Introduction

This is just a minimal installation for Arch Linux.

It has been tested on VMWare and a Real Machine.

## Preparing

Insert Arch ISO and boot it.

## Verify Boot Mode

if the below command lists without error, then the system is booted in UEFI mode. I just tested this mode.

```bash
ls /sys/firmware/efi/efivars
```

## Set Network Time Protocol

```bash
timedatectl set-ntp true
```

## Create Partitions

Enter this command to list all disks and partitions:

```bash
fdisk -l
```

Select the disk you wanna partition it:

```bash
cfdisk /dev/sda
```

Note: I use `sda`. maybe you need to use `nvme0n1` or something like that if you have m.2 disk.

I created 3 partitions like this:

| Device | Size | Type |
|--------|------|------|
| `/dev/sda1` | 512M | Type: EFI System |
| `/dev/sda2` | Remaining Disk Space | Linux root (x86-64) |
| `/dev/sda3` | 2 x System Total Ram | Linux Swap |

Use `New` and `Type` from menu to do that. Then `Write` and `Quit`.

Now format your linux partition:

```bash
mkfs.ext4 /dev/sda2
```

Then make swap on:

```bash
mkswap /dev/sda3
swapon /dev/sda3
```

## Install

Mount linux partition:

```bash
mount /dev/sda2 /mnt
```

Install the base Arch system:

```bash
pacstrap /mnt base linux linux-firmware
```

> I'm not sure which packages are required!

Generate `fstab` file:

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

Chroot into Arch:

```bash
arch-chroot /mnt
```

## Config Network

Use your own hostname:

```bash
hostnamectl set-hostname myarch
```

```bash
touch /etc/hosts
echo 127.0.0.1  localhost >> /etc/hosts
echo 127.0.1.1  myarch >> /etc/hosts
echo ::1  localhost >> /etc/hosts
```

## Install Grub Bootloader

```bash
pacman -S grub efibootmgr
mkdir /boot/efi
mount /dev/sda1 /boot/efi
grub-install --target=x86_64-efi --bootloader-id=GRUB --efi-directory=/boot/efi
grub-mkconfig -o /boot/grub/grub.cfg
```

## Enable DHCPCD Service

```bash
pacman -S dhcpcd
systemctl enable dhcpcd.service
```

## Set Root Password

You need to set root password before finish

```bash
passwd
```

## Finalizing

```bash
exit
shutdown -r now
```
