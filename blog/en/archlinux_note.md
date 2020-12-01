@def title = "Archlinux Install & Config"
@def published = "10 July 2019"
@def tags = ["Tips", "Arch", "Linux"]

\toc

## Arch Linux config backlight

### Get your graphics card folder

```

ls /sys/class/backlight/

```

*note: the folder name depends on your graphics card*

### Change the work directory



```

cd /sys/class/backlight/intel_backlight #maybe your folder name is another.

```

### Edit the file named `brightness`

*note: you can `cat` the `max_brightness` to get the max brightness*

```

vim brightness

```



------------------



## Arch Linux config touchpad

>Driver: libinput

1. Open the config file

```

vim /usr/share/X11/xorg.conf.d/40-libinput.conf

```



*note: maybe you can edit `/etc/X11/xorg.conf.d/40-libinput.conf` after running `ln -s /usr/share/X11/xorg.conf.d/40-libinput.conf /etc/X11/xorg.conf.d/40-libinput.conf`, but I haven't try it.*

2. Edit the section that contains `MatchIsTouchpad "on"` and add the following code:

```

Option "Tapping" "on" # tap-to-click

Option "ClickMethod" "clickfinger" # two-finger click is a context click and three-finger click is a middle click

Option "NaturalScrolling" "true" # reverse scrolling

```



------------------



## Linux running python scripts in the background

Run `nohup python filename.py >filename.out 2>&1 &`

*note: `filename.out` is a log file*

For details, you can refer the [gnu coreutils](https://www.gnu.org/software/coreutils/manual/html_node/nohup-invocation.html#nohup-invocation)

## Linux kill python scripts

```

pgrep -f blabla.py # will give you its pid



pkill -9 -f blabla.py # kills the matching pid

```



------------------



## Arch Linux install MySQL

Since 2013 MariaDB is Arch Linux's default implementation of MySQL. So we can easily install **mariadb** with `pacman`. Of course, you still can install mysql referring to [official guide](https://dev.mysql.com/doc/refman/5.7/en/installing.html).

I choose _mariadb_ due to convenience.



### Step 1: Install the package

Execute this command: `sudo pacman -S mariadb`

### Step 2: Basical config

Run the following command as **root** before starting the mariadb:

```

mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql

```

### Step 3: Start mariadb

```

systemctl start mariadb

mysql_secure_installation //the mariadb default passwd is none, and you can set your passwd

systemctl restart mariadb

```

### Step 4: Log in

To log in as `root` on the MySQL server, use the command: `mysql -u root -p`



------------------



## Arch Linux disable beep

i tried this [arch wiki](https://wiki.archlinux.org/index.php/PC_speaker), but it useless. so I tried this method, luckily, it works.

- Log in as 'root'

- Run:

`echo "blacklist pcspkr" > /etc/modprobe.d/nobeep.conf`

- Reboot



------------------



## Arch Linux reset root passwd

(https://www.ostechnix.com/how-to-reset-or-recover-root-user-password-in-linux/)

i **haven't** try it, so i don't know if it works.



------------------



## Arch Linux Installation Guide on HP Pavilion

My computer details:

>Computer: HP Pavilion laptop

>CPU: Intel i5-8250U

>GPU: NVIDIA MX150

## Pre-installation

### Verify the boot mode

To verify if it is UEFI mode, use this command:

`ls /sys/firmware/efi/efivars`

If it show some details, the system is UEFI. Otherwise, the system may be booted in BIOS or CSM mode.

### Connect to the internet

Because I'm using HP laptop. so i just need connect to wifi with this command `wifi-menu`. If you want to connect to wired network, reference official guide about [dhcpcd](https://wiki.archlinux.org/index.php/Dhcpcd#Installation)

You can check this connection by `ping www.archlinux.org`.

### Update the system clock

Use ` timedatectl set-ntp true` to ensure the system clock is accurate, to check the service status, use `timedatectl status`.



### Partition the disks

Use `lsblk` or `fdisk -l` to vertified device.

First, run `fdisk /dev/sda`（note: if your device is /dev/sdX, run `fdisk /dev/sdX`), and you will enter the *fdisk* dialog.

Next enter `m` you will get help details. If you use a separate hard drive. enter `g` to format it as GPT.

Next, enter `n` to create new partitions, enter `w` to exit if you finished.



This is my partition layout:



Mount points | Partition | Partition Type | Size

|----|---|---|---|

/mnt | /dev/sda3 | linux filesystem |50G

/mnt/boot | /dev/sda2 | linux filesystem|500M

/mnt/boot/EFI | /dev/sda1 | EFI system partition|500M

[swap] | /dev/sda4 | linux swap|16G

/mnt/home | /dev/sda5|linux filesystem|300G



### Format the partitions

If the EFI partition is on /dev/sdX0, run:

`mkfs.fat -F32 /dev/sdX0`



If the root partition is on* /dev/sdX1* and will contain the ext4 file system, run:

`mkfs.ext4 /dev/sdX1`



If you created a partition for swap, initialize it with mkswap:

```

mkswap /dev/sdX2

swapon /dev/sdaX2

```



Run these commands if you use my layout:

```

mkfs.fat -F32 /dev/sda1

mkfs.ext4 /dev/sda2

mkfs.ext4 /dev/sda3

mkfs.ext4 /dev/sda5

mkswap /dev/sda4

swapon /dev/sda4

```

### Mount the file systems

Run these commands if you use my layout:

```

mount /dev/sda3 /mnt

mkdir /mnt/boot

mount /dev/sda2 /mnt/boot

mkdir /mnt/boot/EFI

mount /dev/sda1 /mnt/EFI

mkdir /mnt/home

mount /dev/sda5 /mnt/home

```

## Installation

### Select mirrors

- Open mirrorlist file by `nano /etc/pacman.d/mirrorlist`

- Uncomment your selected server.

- save changes

### Install the base packages

Use `pacstrap /mnt base base-devel`

## Configure the system

### Generate an fstab file

- Use `genfstab -U /mnt >> /mnt/etc/fstab`

- Check it in case of errors: `cat /mnt/etc/fstab`

### Into new system

Run `arch-chroot /mnt`

### Timezone and Location

- Set Timezone:

`ln -sf /usr/share/zoneinfo/Region/City /etc/localtime`



- Set the hardware clock:

`hwclock --systohc`



- Set location

1. Uncomment en_US.UTF-8 UTF-8 and other needed locales in **/etc/locale.gen**, and generate it:

```

nano /etc/locale.gen

locale-gen

```

By the way, the new system doesn't have `vim`.



2. Create the `locale.conf` and set `LANG`variable:

```

nano /etc/locale.conf

LANG= en_US.UTF-8 UTF-8

```

### Network Config

- Create the *hostname* file and enter your hostname: ` nano /etc/hostname`.

- Add matching entries to *hosts*:

```

127.0.0.1 localhost

::1 localhost

127.0.1.1 myhostname.localdomain myhostname

```

### Set root password

Run `passwd`



### Enable microcode updates

```

pacman -S intel-ucode # for intel CPU

pacman -S amd-ucode # for amd CPU

```

### Install and set Bootloader

I use [grub](https://en.wikipedia.org/wiki/GNU_GRUB), so:

- Install related packages:

`pacman -S grub dosfstools efibootmgr`

- Install grub

`grub-install --target=x86_64-efi --efi-directory=/boot/EFI --recheck`

- Use the grub-mkconfig tool to generate /boot/grub/grub.cfg:

`grub-mkconfig -o /boot/grub/grub.cfg`



### Install Wireless LAN tools

I use `wifi-menu` to connect to wifi, so I need this packages:

`pacman -S wpa_supplicant dialog iw`

### Add a user

- Add a user

`useradd -m -g users -s /bin/bash username`

- Set password for user

`passwd username`

- Privilege escalation(sudo)

`nano /etc/sudoers` and add `username ALL=(ALL) ALL` below `root ALL=(ALL) ALL`



## Post-Installation

### Install display server

Run the minimal command: `pacman -S xorg-server`.

### Install display driver

Because I use*NVIDIA MX150* graphics card, I use this `xf86-video-nouveau`driver. For details you can referrence [this](https://wiki.archlinux.org/index.php/NVIDIA). If you use *amd card*, you can read [this](https://wiki.archlinux.org/index.php/Xorg#AMD)



------------------



__Thank you for reading!__

I will continue to update this article later if I find something.


