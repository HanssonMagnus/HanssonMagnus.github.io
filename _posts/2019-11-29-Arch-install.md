---
layout: post
title: Arch install
---

#### Introduction
Arch is a pleasant distribution since it let's you take control of the installation process. You can install what you want to install, and do not need to run a system with bloatware. This means that you need to do some configuration yourself.

#### Resources
When installing Arch always use the official documentation, the process chan*ges.
* [Arch wiki](https://wiki.archlinux.org/)
* [Installation guide](https://wiki.archlinux.org/index.php/Installation_guide)
* [Network configuration](https://wiki.archlinux.org/index.php/Network_configuration)
* [Users and groups](https://wiki.archlinux.org/index.php/Users_and_groups)

#### Set up your layout
Keyboard,
```
loadkeys sv-latin1
```
If you want to e.g. vertically rotate your screen,
```
echo 1 > /sys/class/graphics/fbcon/rotate_all
```

#### UEFI check
UEFI is a replacement for BIOS and if your motherboard is configured for it you have files here,
```
ls /sys/firmware/efi/efivars
```

#### Check internet connection
If you have a wifi connection you can run "wifi-menu", otherwise ethernet is automatically working. Check connection,
```
ping magnushansson.xyz
```

#### Sync time
```
timedatectl set-ntp true
timedatectl status
```

#### Partition the disks
Look at your disk structure,
```
lsblk
```

There are several ways to set up a partition structure, and one could argue that it's better to have one for boot, one for root, one for home and so on, if something goes wrong in the system.

Usually a regular partition structure is,

Mount point | Partition | Size
--- | --- | ---
/mnt | Linux | Remainder of space
[SWAP] | Linux Swap | +12G

The swap partition should be around x1.5 of your memory. To partition the disk you can use fdisk or cfdisk, in which cfdisk is graphical and more intuitive,

```
cfdisk
```
Select dos,then primary partition and bootable for the Linux partition and primary for the swap. Change the swap partition to be of type "Linux swap". Check with "lsblk".

If you do not get a menu in "cfdisk" to select label of the disk you may want to change the label to dos (given that you want to use dos and not gpt for UEFI system),

```
parted /dev/sda
mklabel msdos
quit
```

#### Format partitions

```
mkfs.ext4 /dev/sda1

mkswap /dev/sda2
swapon /dev/sda2
```

#### Mount the file systems
```
mount /dev/sda1 /mnt
```

#### Installing the system
```
vim /etc/pacman.d/mirrorlist
```
Pick mirrors that are close to you. The higher up one the list a mirror is the higher priority it is given. This file will be copied in the installation process.

Install the distribution and any other programs that you want,

```
pacstrap /mnt base base-devel linux linux-firmware
```

#### Configure the system
Generate file system table file,

```
genfstab -U /mnt >> /mnt/etc/fstab
vim /mnt/etc/fstab
```
You can see in the file that the option -U selected the UUID option and the /dev/sda is commented out.

#### Change to root
Change root into your new system,
```
arch-chroot /mnt
```

#### Set time zone
```
ls /usr/zoneinfo
ln -sf /usr/share/zoneinfo/Europe/Stockholm /etc/localtime
hwclock --systohc
```

#### Localization
Generate the locales and edit the config,
```
locale-gen

vim /etc/locale.conf
```
Write "LANG=en_US.UTF-8". Make the keyboard layout permanent,

```
vim /etc/vconsole.conf
```
write "KEYMAP=sv-latin1".

#### Network config
```
vim /etc/hostname
```
Write your hostname, e.g. "ArchServer".

```
vim /etc/hosts
```
Edit this file with the following information,

```
127.0.0.1       localhost
::1             localhost
127.0.1.1	    myhostname.localdomain  myhostname
```

#### Setting root password
Set password for the user that you are logged in as, we are logged in as root,
```
passwd
```

#### Network setup
One thing that is not in the Arch installation guide is configuring the network internet settings. If you do not do this you will not have an internet connection when booting into your new system.

```
systemctl enable dhcpcd
```

or

```
pacman -S networkmanager
systemctl enable NetworkManager
```

#### Add user
```
useradd -m magnus
passwd magnus
```

Sudo is not installed by default as can be seen,
```
whereis sudo
pacman -S sudo
```

Add user to groups, the wheel group gets you root access in Arch,

```
usermod -aG wheel,audio,video,optical,storage magnus
groups magnus
visudo
```
Uncomment "# %wheel ALL=(ALL) ALL".

#### Install bootloader
```
pacman -S grub
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
```

#### Exit root and shutdown
```
exit
shutdown
```








