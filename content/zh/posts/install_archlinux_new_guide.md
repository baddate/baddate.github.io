---
title: "ArchLinux最新安裝教程"
date: 2020-07-05
description:
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
series:
-
-url:
    /zh/tips/archlinux_installation_zh
tags:
- Tips
- ArchLinux
- Linux
- Installation
categories:
- 2020-07

image:
--- 

# 前言
自从3月份从Arch换到Fedora系统之后，刚开始感觉还不错，因为大部分东西都已经帮你搞定了，自己只要会用就可以了，但是用的久了，感觉Fedora的软件包管理器真心不如Arch的pacman好用，有些中文软件在仓库中根本找不到，自己配置起来很麻烦，但是如果用Arch的话，构建起来就很简单了，所以最终还是决定重新拥抱Arch。
刚一开始我还准备按照我之前总结的安装记录进行安装，毕竟当时记录的还是很详细的，但是没想到刚进入Live系统就碰到了一个难题：`wifi-menu`命令不存在！ ！ EXCUSE ME?? 我明明验证过镜像完整性的，没问题啊，这是怎么回事？还好，机智的我知道这里面有问题，查阅了一点资料之后就发现(这里不得不吹一波Arch的用户社区，真心给力，盲猜是最活跃的Linux发行版社区)原来是官方弃用了，改用`iwd`+`systemd-networkd`来管理网络了，莫得办法，那就换吧，查了查iwd的基本命令使用，发现还挺简单的，一次配置，永久使用，比` wifi-menu`用起来要流畅许多！

---
archlinux的iso镜像文件自2020.06.01开始改了很多东西，大概就这几个方面：
1. archiso默认的shell是zsh，不再是bash了。
2. archlinux摒弃了之前的`wifi-menu`，改为使用iwd来管理网络。
3. 对于镜像源的管理，采用reflector进行管理，终于不用去手动更改镜像源的位置了，使用reflector一条命令就搞定了。

# 正文
## 必需条件
1. 一台可以使用的电脑
2. 一个4G以上内存的USB存储器
3. 一个可以查看本文的电子设备:)

## 制作USB启动盘
1. 首先在[这里](https://www.archlinux.org/download/)找一个延迟低的官方镜像站下载ArchLinux的iso镜像，我下载的是最新版本，参数如下:
- Current Release: 2020.07.01
- Included Kernel: 5.7.6
- ISO Size: 647.0 MB

2. 在镜像站下载PGP签名，然后使用`gpg`验证签名确保镜像完整性以及安全性，命令如下: `gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso. sig`

3. 然后使用磁盘烧录工具将镜像写入你的USB存储器。 Linux系统建议使用`dd`这个神器。 Windows系统的话，建议使用开源软件`rufus`，最好不要使用那个UltralISO，很容易出问题。

## 开始安装

### 验证启动模式
从USB进入Live环境，然后验证你的机器的启动模式是UEFI还是BIOS：

```bash
ls /sys/firmware/efi/efivars
```

### 连接网络

*前面已经说过了，Archiso的默认的无线网络管理改为`iwd`了，所以这里就讲一下如何使用iwd连接wifi*
1. 在终端中输入`iwctl`进入iwd提示符：
```bash
[root@archiso~] iwctl
[iwd#]
```
2. 在`[iwd#]`中输入`device list`查询机器的网卡设备。
3. 使用以下命令查询附近可用的wifi网络：
```bash
[iwd#] station <devicename> scan
[iwd#] station <devicename> get-networks # 显示扫描的结果
```
4. 在提示符中输入`station <devicename> connect <wifi-ssid>`连接wifi网络，如果wifi加密，会提示你输入密码

### 更新系统时间
使用`timedatectl`命令来确保时间是同步的：
```bash
timedatectl set-ntp true
timedatectl status # 确保设置成功
```
### 磁盘分区

首先使用`lsblk`或者其他磁盘工具（例如`fdisk`）查看磁盘设备：

```bash
lsblk
// or
fdisk -l
```

然后使用`fdisk`或者`cfdisk`创建磁盘分区，这里有一点要注意的是，在创建分区的时候， __必需要确保有一个root分区`/`__ ，此外，对于UEFI模式的设备还需要一个额外的EFI系统分区，我的分区方式如下：

|挂载点|分区类型|大小|
|------|--------|----|
|/mnt/efi|EFI system partition|1G|
|/mnt|Linux x86-64 root|32G|
|[swap]|Linux swap|16G|
|/home|Linux home|Remainder of the device|

无论是`fdisk`还是`cfdisk`，用起来都很简单，所以这里不再具体介绍了，想要具体了解的可以查阅[fdisk官方文档](https://wiki.archlinux.org/index.php/Fdisk)、[cfdisk官方文档](https://wiki.archlinux.org/index.php/Cfdisk)。对于新人小白，个人推荐`cfdisk`，有一个伪图形界面，很容易上手。

### 格式化分区
按照上面的步骤建立好分区之后，我们需要将每个分区用对应的文件系统进行格式化。

对于root分区、home分区等直接使用`ext4`文件系统进行初始化：
```bash
mkfs.ext4 /dev/sda2
mkfs.ext4 /dev/sda4
```

如果你的机器是UEFI启动模式，使用以下命令初始化EFI系统分区：
```bash
mkfs.fat -F32 /dev/sda1
```

对于交换分区，不能使用上述命令进行格式化，而需要使用`mkswap`将其初始化：
```bash
mkswap /dev/sda3
swapon /dev/sda3
```

### 挂载分区

首先挂载`root`分区：
```bash
mount /dev/sda2 /mnt
```

对于其他分区（swap分区除外，不需要），需要自己手动创建挂载点：
```bash
mkdir /mnt/efi
mount /dev/sda1 /mnt/efi
mkdir /mnt/home
mount /dev/sda4 /mnt/home
```

### 选择镜像源
文件`/etc/pacman.d/mirrorlist`定义了软件包会从哪个镜像源下载。在 Live 启动的系统上，所有的镜像都被启用。在列表中越前的镜像在下载软件包时有越高的优先权。你可以相应的修改文件`/etc/pacman.d/mirrorlist`，并将地理位置最近的镜像源挪到文件的头部来保证下载速度。而且这个文件接下来还会被 pacstrap 拷贝到新系统里，在这里设置好之后就可以一劳永逸了。

>这一步其实现在可以省略了，因为现在在live环境中使用`reflector`进行镜像的管理，貌似你一连接网络，live系统会自动执行reflector命令来帮你选择镜像源，默认的是根据下载速率进行排序，感兴趣的小伙伴可以自己确认一下（连接网络之前备份一下`/etc/pacman.d/mirrorlist`文件，然后连接网络之后，使用`diff`对比前后两个文件的差异）。

### 安装必需软件包

使用archiso内置的脚本`pacstrap`安装一些基本的软件包、内核及常规的硬件固件：
```bash
pacstrap /mnt base linux linux-firmware
```

>这里要注意的是，上面的命令并不包括所有的基本程序，如网络管理程序、文本编辑器等，如果你想安装这些程序，可以将名字添加到`pacstrap`后，并用空格隔开。你也可以在Chroot进新系统后使用`pacman`手动安装软件包或组。

到这一步，理论上其实新系统已经安装完了，不过还无法正常使用:)所以需要进行配置以正常使用。

### 配置系统

#### 生成fstab文件
用以下命令生成`fstab`文件，其中`-U`选项用来设置UUID：
```
genfstab -U /mnt >> /mnt/etc/fstab
```

然后使用`cat /mnt/etc/fstab`命令检查以下文件是否正确（每个分区占一行）

### 进入到安装的系统
```bash
arch-chroot /mnt
```

### 安装文本编辑器
现在的新系统连默认的文本编辑器`nano`都没有了，所以需要自己手动安装一个，不然后面的一些配置无法实现，所以我选择最强的`vim`：
```bash
pacman -S vim
```

### 时区
使用下面的命令设置时区：
```bash
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
```
然后使用`hwclock`生成`/etc/adjtime`文件：
```
hwclock --systohc
```

### 本地化设置
>本地化的程序与库若要本地化文本，都依赖[Locale](https://wiki.archlinux.org/index.php/Locale)，后者明确规定地域、货币、时区日期的格式、字符排列方式和其他本地化标准等等。在下面两个文件设置：`locale.gen`与`locale.conf`。

1. 首先编辑`/etc/locale.gen`文件，然后将需要的地区的注释移除，建议将`en_US UTF-8`和`zh_CN UTF-8`都取消注释。
2. 执行`locale-gen`命令生成locale。
3. 创建`locale.conf`文件并编辑`LANG`这一变量（将系统locale 设置为`en_US.UTF-8`，系统的`Log`就会用英文显示，这样更容易问题的判断和处理。）：
```bash
LANG=en_US.UTF-8
```
*这里最好不要设置为中文locale，会导致TTY乱码*

### 网络设置

1. 创建`/etc/hostname`文件设置主机名
2. 配置`/etc/hosts`文件，将以下内容添加进去：
```
127.0.0.1 localhost
::1 localhost
127.0.1.1 myhostname.localdomain myhostname
```
*note: 如果系统有一个永久的 IP 地址，请使用这个永久的 IP 地址而不是 127.0.1.1。 *

### 设置root密码

使用`passwd`命令设置root密码即可。

这里不得不说一句，使用arch是真的让人舒服的一批，不会出现一些令人无语的问题，我还记得第一次使用linux的时候，用的是fedora，当时想要使用`su `进入root，但是发现需要密码，但是我不记得安装的时候设置过root密码啊，后来查阅了一些资料才发现，fedora并没有设置root密码，需要自己手动设置（使用`sudo passwd root`）。

### 安装及配置引导程序
>安装引导程序之后才能进入系统

我用的引导程序是[GRUB](https://wiki.archlinux.org/index.php/GRUB)，首先安装必要的软件包：
```bash
pacman -S grub efibootmgr
```

这里详细介绍一下UEFI系统如何安装配置GRUB：
1. 首先使用以下命令安装到系统：
```bash
grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=ArchLinux
```
note: 因为我的EFI分区在`/efi`目录下，所以上述命令的`--efi-directory`参数就设置为`/efi`
2. 使用`grub-mkconfig `生成grub配置文件：`grub-mkconfig -o /boot/grub/grub.cfg`

### 安装wifi网络管理工具
如果你不安装的话，新系统是无法联网的哦
我用的是`iwd`，简单方便又强大
```bash
pacman -S iwd
```
### 重启
1. 输入`exit`或按`Ctrl+d`退出`chroot`环境
2. 用`umount -R /mnt`手动卸载被挂载的分区
3. 执行`reboot`重启系统

## 再配置新系统
>因为还没有设置普通用户，所以重启后需要使用root来进入新系统

### 连接网络
_如果你没有安装wifi网络管理工具，或者你安装的工具需要gui支持，那么恭喜你，你的系统还是无法联网，恭喜你可以重新体验一次完整的archlinux安装流程:)_

如果你按照我上面写的安装了wifi工具`iwd`，那么你还需要做一些额外的工作才能正常使用它：
启动iwd服务，为了以后的使用，建议直接设置为开机自启动，详细方法可以参考[systemd](https://wiki.archlinux.org/index.php/Systemd#Using_units)
```bash
systemctl start iwd.service # 启动服务
systemctl enable iwd.service # 开机自启动服务
```

你是不是以为这样就能使用iwd来连接wifi了？那你就大错特错了， __你还需要启动`systemd-networkd.service`和`systemd-resolved.service`才行，因为新系统默认不会自启动这两个服务，需要你手动开启__ (血的教训换来的经验，请珍惜)

到这里你才能正常使用`iwd`这个工具，具体使用方法可以参考[前文](#连接网络)

### 安装显示服务器
使用下面的命令安装开源的xorg
```bash
pacman -S xorg
```
### 安装显卡驱动
根据自己的显卡配置来选择安装即可。

我的机器配置为intel集成显卡+NVIDIA独立显卡：

对于intel显卡，我安装的是官方的`xf86-video-intel`驱动：
```bash
pacman -S xf86-video-intel
```

对于NVIDIA显卡，我安装的是开源驱动`nouveau`：
```bash
pacman -S mesa xf86-video-nouveau
```

### 安装桌面环境
我使用的是`XFCE`，用起来不错，之所以不想选择KDE是因为它太像windows了，而gnome感觉太臃肿了，所以最终选择了XFCE，轻便又美观。
使用以下命令安装xfce：
```bash
pacman -S xfce4
```
安装完成后还需要配置xinitrc文件来保证开机直接启动

### 安装显示管理器
由于`xfce4`包里没有显示管理器(DM)，所以还需要自己选择一个DM进行安装，我选择的是`lightdm`：
```bash
pacman -S lightdm lightdm-gtk-greeter
```

编辑`/etc/lightdm/lightdm.conf`设置`lightdm-gtk-greeter`为默认的greeter：
```/etc/lightdm/lightdm.conf
[Seat:*]
...
greeter-session=lightdm-gtk-greeter
...
```

设置为开机自启动
```bash
systemctl enable lightdm.service
```

### 添加普通用户
1. 使用以下命令添加一个用户
```bash
useradd -m -G "附加组" -s "登陆shell" "用户名"
```
其中：
- `-m`/`--create-home`：创建用户主目录/home/[用户名]
- `-G`/`--groups`：用户要加入的附加组列表；使用逗号分隔多个组，不要添加空格；如果不设置，用户仅仅加入初始组。
- `-s`/`--shell`：用户默认登录shell的路径

2. 赋予用户root权限
- 安装sudo软件包: `pacman -S sudo`
- 在`/etc/sudoers`文件中的`root ALL=(ALL) ALL`行下添加`yourname ALL=(ALL) ALL`

----------

至此，带有图形界面的arch系统安装完成了，已经可以基本使用了，如果有其他需求，自己自行进行配置即可。

## 个性化系统

### 安装中文输入法
archlinux中文输入法框架有`fcitx`/`ibus`/`fcitx5`，其中`fcitx5`貌似是刚刚出现的，之前没见过，所以我就试了试，安装配置起来还挺简单的：
1. 安装`fcitx5`软件包
2. 创建`~/.pam_environment`并配置环境变量：
```~/.pam_environment
GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE DEFAULT=fcitx
XMODIFIERS DEFAULT=@im=fcitx
```
3. 安装中文输入法引擎`fcitx5-rime`
4. 安装输入法模块来保证输入法在各种程序中使用：`pacman -S fcitx5-gtk fcitx5-qt5`
5. 启动fcitx5，右键fcitx5的系统托盘图标，点击`configure`添加rime输入法
6. `ctrl`+`space`键测试是否可以正常使用

### 安装字体
```bash
pacman -S ttf-dejavu # 英文字体
pacman -S wqy-microhei # 中文字体
```

### 安装zsh
有一说一，默认的bash真的有点丑，所以安装一个好看的shell很有必要，比如说zsh。安装zsh的话，我推荐使用[oh my zsh](https://github.com/ohmyzsh/ohmyzsh)这个框架个性化zsh，配置zsh起来简单方便。

1. 安装zsh
2. 使用curl安装ohmyzsh，这个脚本还可以帮你自动更改默认的shell
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

具体如何配置zsh，可以参考ohmyzsh的[官方wiki](https://github.com/ohmyzsh/ohmyzsh/wiki)。
