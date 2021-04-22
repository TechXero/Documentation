# -/ Compile Fresh ArchISO :

1- sudo pacman -S archiso<br />
2- Copy over releng folder from /usr/share/archiso<br />
3- Create "work" & "out" folders<br />
4- sudo mkarchiso -v -w work -o out releng<br />
5- sudo rf out && sudo rf work<br />

# Arch Linux Installation

Based on the link: [Installation de base (French)](https://wiki.archlinux.fr/installation#Pr.C3.A9paration_avant_l.27installation)

As written,
    
    It is strongly discouraged to use tutorials when installing Arch Linux. Tutorials are rarely updated, which conflicts with the constant evolution of any rolling release.
    The use of an outdated or incorrect tutorial can lead to serious malfunctions.

These notes are mine and worked in 2021 using the ISO.

## Add a root password to allow SSH connection later.
    root@archiso ~ # passwd

## Enable SSH if necessary
First, use the `ip a` command to see if you are connected to the network.

    root@archiso ~ # ip a
    ...snip...
    2: enp0s31f6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether d4:81:d7:98:ae:cf brd ff:ff:ff:ff:ff:ff
        inet 192.168.1.65/24 brd 192.168.1.255 scope global noprefixroute enp0s31f6
           valid_lft forever preferred_lft forever
    ...snip...    

Then, enable the SSH daemon `systemctl start sshd`

## Now connect to your computer with a ssh client (putty, mobaXterm...)
It will ease the copy/paste commands and avoid the keyboard layout issues.

## Update the timezone and update the time
    root@archiso ~ # timedatectl set-timezone Europe/Paris
    root@archiso ~ # timedatectl set-ntp true
    root@archiso ~ # timedatectl
                   Local time: Sat 2019-08-17 20:00:00 CEST
               Universal time: Sat 2019-08-17 18:00:00 UTC
                     RTC time: Sat 2019-08-17 18:00:00
                    Time zone: Europe/Paris (CEST, +0200)
    System clock synchronized: no
                  NTP service: active
              RTC in local TZ: no

## Disk partitioning and preparation
I have 2 disks: 1 NVMe SSD and 1 HDD (SATA I guess)

    root@archiso ~ # blkid
    /dev/nvme0n1: PTUUID="f5b518ad-d6b4-6b4f-9a67-4b35a202e7ad" PTTYPE="gpt"
    /dev/sda: PTUUID="4488b418-a0fe-6441-ad98-42a02dbb15f2" PTTYPE="gpt"

I'm using UEFI so I'm using this partition scheme:

    root@archiso ~ # fdisk /dev/sda
    
    Welcome to fdisk (util-linux 2.34).
    Changes will remain in memory only, until you decide to write them.
    Be careful before using the write command.
    
    Command (m for help): g
    Created a new GPT disklabel (GUID: 10C217A8-7AFB-A54A-A8C8-4FC9638369B9).

    Command (m for help): n
    Partition number (1-128, default 1):
    First sector (34-1953525134, default 2048):
    Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-1953525134, default 1953525134):
    Created a new partition 1 of type 'Linux filesystem' and of size 931.5 GiB.
    
    Command (m for help): w
    The partition table has been altered.
    Calling ioctl() to re-read partition table.
    Syncing disks.

    root@archiso ~ # fdisk /dev/nvme0n1
    
    Welcome to fdisk (util-linux 2.34).
    Changes will remain in memory only, until you decide to write them.
    Be careful before using the write command.
    
    Command (m for help): g
    Created a new GPT disklabel (GUID: A877DA14-A4C0-5047-917F-7E1891F5EF27).
    
    Command (m for help): n
    Partition number (1-128, default 1):
    First sector (34-500118158, default 2048):
    Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-500118158, default 500118158): +500M
    Created a new partition 1 of type 'Linux filesystem' and of size 500 MiB.
    
    Command (m for help): n
    Partition number (2-128, default 2):
    First sector (1026048-500118158, default 1026048):
    Last sector, +/-sectors or +/-size{K,M,G,T,P} (1026048-500118158, default 500118158):
    Created a new partition 2 of type 'Linux filesystem' and of size 238 GiB.
    
    Command (m for help): w
    The partition table has been altered.
    Calling ioctl() to re-read partition table.
    Syncing disks.

    root@archiso ~ # blkid
    /dev/nvme0n1p1: PARTUUID="584bf9ab-a29e-c14a-848c-40c37bf42825"  --> Will be /boot/efi
    /dev/nvme0n1p2: PARTUUID="8e04317e-edbd-3440-8fdc-f83dd495ee72"  --> Will be /
    /dev/sda1: PARTUUID="6fbbf30a-3d3e-1548-8c32-99794b69adcd"       --> Will be /home

    root@archiso ~ # mkfs.vfat -F32 /dev/nvme0n1p1
    mkfs.fat 4.1 (2017-01-24)

    root@archiso ~ # mkfs.ext4 /dev/nvme0n1p2
    mke2fs 1.45.3 (14-Jul-2019)
    Discarding device blocks: done
    Creating filesystem with 62386513 4k blocks and 15597568 inodes
    Filesystem UUID: 131c22d2-c99f-4661-b085-5d34d76ac7b9
    Superblock backups stored on blocks:
            32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
            4096000, 7962624, 11239424, 20480000, 23887872
    
    Allocating group tables: done
    Writing inode tables: done
    Creating journal (262144 blocks): done
    Writing superblocks and filesystem accounting information: done

    root@archiso ~ # mkfs.ext4 /dev/sda1
    mke2fs 1.45.3 (14-Jul-2019)
    Creating filesystem with 244190385 4k blocks and 61054976 inodes
    Filesystem UUID: 1efd21cc-fb19-42dc-8dd0-b9c993f9fa36
    Superblock backups stored on blocks:
            32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
            4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968,
            102400000, 214990848
    
    Allocating group tables: done
    Writing inode tables: done
    Creating journal (262144 blocks): done
    Writing superblocks and filesystem accounting information: done

## Partition mount
    root@archiso ~ # mount /dev/nvme0n1p2 /mnt
    root@archiso ~ # mkdir -p /mnt/boot/efi && mount -t vfat /dev/nvme0n1p1 /mnt/boot/efi
    root@archiso ~ # mkdir /mnt/home && mount /dev/sda1 /mnt/home

## Installation
    root@archiso ~ # pacstrap /mnt base base-devel wireless_tools wpa_supplicant dialog git openssh

## System configuration
    root@archiso ~ # genfstab -U -p /mnt >> /mnt/etc/fstab
    root@archiso ~ # arch-chroot /mnt
    
    [root@archiso /]# echo ArchLinux > /etc/hostname
    [root@archiso /]# echo '127.0.1.1 ArchLinux.localdomain ArchLinux' >> /etc/hosts
    [root@archiso /]# ln -sf /usr/share/zoneinfo/Europe/Paris /etc/localtime
    [root@archiso /]# vi /etc/mkinitcpio.conf
    ...snip...
    MODULES=(i915 amdgpu)
    ...snip...
      
    [root@archiso /]# mkinitcpio -p linux
    [root@archiso /]# passwd
    New password:
    Retype new password:
    passwd: password updated successfully

## Unmount and poweroff
    root@archiso ~ # cd && umount -R /mnt
    root@archiso ~ # poweroff

Remove the CD/DVD/USB and power on the computer to boot on Arch Linux

# Add ArchUser user
    [root@ArchLinux ~]# mkdir /home/archuser
    [root@ArchLinux ~]# useradd archuser && gpasswd -a archuser users
    [root@ArchLinux ~]# passwd archuser
    [root@ArchLinux ~]# visudo
    ...snip...
    root ALL=(ALL) ALL
    bastien ALL=(ALL) ALL
    ...snip...

# Enable internet
    [root@ArchLinux ~]# ip link show
    [root@ArchLinux ~]# ip link set dev enp0s31f6 up
    [root@ArchLinux ~]# dhcpcd enp0s31f6
    [root@ArchLinux ~]# systemctl enable dhcpcd

# Enable ssh if needed
    [root@ArchLinux ~]# echo 'PermitRootLogin yes' >> /etc/ssh/sshd_config
    [root@ArchLinux ~]# systemctl start sshd

# Install XOrg
    [bastien@ArchLinux ~]# sudo pacman -S --noconfirm xorg-server xorg-font-util xorg-setxkbmap xorg-xkbutils xorg-xauth xorg-xinput xorg-xinit xkeyboard-config xf86-video-amdgpu xf86-video-ati xf86-video-intel
    [bastien@ArchLinux ~]$ cp /etc/X11/xinit/xinitrc ~/.xinitrc

# Install KDE Plasma + SDDM
    [bastien@ArchLinux ~]$ sudo pacman -S --noconfirm plasma-desktop sddm sddm-kcm kscreen
    [bastien@ArchLinux ~]$ vi ~/.xinitrc
    #!/bin/sh
    
    userresources=$HOME/.Xresources
    usermodmap=$HOME/.Xmodmap
    sysresources=/etc/X11/xinit/.Xresources
    sysmodmap=/etc/X11/xinit/.Xmodmap
    
    # merge in defaults and keymaps
    
    if [ -f $sysresources ]; then
        xrdb -merge $sysresources
    fi
    
    if [ -f $sysmodmap ]; then
        xmodmap $sysmodmap
    fi
    
    if [ -f "$userresources" ]; then
        xrdb -merge "$userresources"
    fi
    
    if [ -f "$usermodmap" ]; then
        xmodmap "$usermodmap"
    fi
    
    [root@ArchLinux ~]# sddm --example-config > /etc/sddm.conf
    [root@ArchLinux ~]# vi /etc/sddm.conf
    [root@ArchLinux ~]# systemctl enable sddm
    [root@ArchLinux ~]# systemctl set-default -f graphical.target

# Install missing firmware
    [bastien@ArchLinux Downloads]$ git clone https://aur.archlinux.org/aic94xx-firmware.git
    [bastien@ArchLinux Downloads]$ cd aic94xx-firmware
    [bastien@ArchLinux aic94xx-firmware]$ makepkg -sri --noconfirm
    
    [bastien@ArchLinux Downloads]$ git clone https://aur.archlinux.org/wd719x-firmware.git
    [bastien@ArchLinux Downloads]$ cd wd719x-firmware
    [bastien@ArchLinux wd719x-firmware]$ makepkg -sri --noconfirm
    
    [bastien@ArchLinux ~]$ sudo mkinitcpio -p linux
    
# Install the software I use
    [bastien@ArchLinux ~]$ sudo pacman -S --noconfirm vivaldi vivaldi-ffmpeg-codecs meld dolphin

# ----> E.O.F
