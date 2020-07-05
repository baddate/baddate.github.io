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


自從3月份從Arch換到Fedora系統之後，剛開始感覺還不錯，因爲大部分東西都已經幫你搞定了，自己只要會用就可以了，但是用的久了，感覺Fedora的軟件包管理器真心不如Arch的pacman好用，有些中文軟件在倉庫中根本找不到，自己配置起來很麻煩，但是如果用Arch的話，構建起來就很簡單了，所以最終還是決定重新擁抱Arch。
剛一開始我還準備按照我之前總結的安裝記錄進行安裝，畢竟當時記錄的還是很詳細的，但是沒想到剛進入Live系統就碰到了一個難題：`wifi-menu`命令不存在！！ EXCUSE ME?? 我明明驗證過鏡像完整性的，沒問題啊，這是怎麼回事？ 還好，機智的我知道這裏面有問題，查閱了一點資料之後就發現(這裏不得不吹一波Arch的用戶社區，真心給力，盲猜是最活躍的Linux發行版社區)原來是官方棄用了，改用`iwd`+`systemd-networkd`來管理網絡了，莫得辦法，那就換吧，查了查iwd的基本命令使用，發現還挺簡單的，一次配置，永久使用，比`wifi-menu`用起來要流暢許多！
# TL;DR
archlinux的iso鏡像文件自2020.06.01開始改了很多東西，大概就這幾個方面：
1. archiso默認的shell是zsh，不再是bash了。
2. archlinux摒棄了之前的`wifi-menu`，改爲使用iwd來管理網絡。
3. 對於鏡像源的管理，採用reflector進行管理，終於不用去手動更改鏡像源的位置了，使用reflector一條命令就搞定了。

# 正文
## 必需條件
1. 一臺可以使用的電腦
2. 一個4G以上內存的USB存儲器
3. 一個可以查看本文的電子設備:)

## 製作USB啓動盤
1. 首先在[這裏]()找一個延遲低的官方鏡像站下載ArchLinux的iso鏡像，我下載的是最新版本，參數如下: 
- Current Release: 2020.07.01
- Included Kernel: 5.7.6
- ISO Size: 647.0 MB

2. 在鏡像站下載PGP簽名，然後使用`gpg`驗證簽名確保鏡像完整性以及安全性，命令如下: `gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig`

3. 然後使用磁盤燒錄工具將鏡像寫入你的USB存儲器。Linux系統建議使用`dd`這個神器。Windows系統的話，建議使用開源軟件`rufus`，最好不要使用那個UltralISO，很容易出問題。

## 開始安裝

### 驗證啓動模式
從USB進入Live環境，然後驗證你的機器的啓動模式是UEFI還是BIOS：

```bash
ls /sys/firmware/efi/efivars
```

### 連接網絡

*前面已經說過了，Archiso的默認的無線網絡管理改爲`iwd`了，所以這裏就講一下如何使用iwd連接wifi*
1. 在終端中輸入`iwctl`進入iwd提示符：
```bash
[root@archiso~] iwctl
[iwd#]
```
2. 在`[iwd#]`中輸入`device list`查詢機器的網卡設備。
3. 使用以下命令查詢附近可用的wifi網絡：
```bash
[iwd#] station <devicename> scan
[iwd#] station <devicename> get-networks # 顯示掃描的結果
```
4. 在提示符中輸入`station <devicename> connect <wifi-ssid>`連接wifi網絡，如果wifi加密，會提示你輸入密碼

### 更新系統時間
使用`timedatectl`命令來確保時間是同步的：
```bash
timedatectl set-ntp true
timedatectl status # 確保設置成功
```
### 磁盤分區

首先使用`lsblk`或者其他磁盤工具（例如`fdisk`）查看磁盤設備：

```bash
lsblk
// or
fdisk -l 
```

然後使用`fdisk`或者`cfdisk`創建磁盤分區，這裏有一點要注意的是，在創建分區的時候， __必需要確保有一個root分區`/`__ ，此外，對於UEFI模式的設備還需要一個額外的EFI系統分區，我的分區方式如下：

|掛載點|分區類型|大小|
|------|--------|----|
|/mnt/efi|EFI system partition|1G|
|/mnt|Linux x86-64 root|32G|
|[swap]|Linux swap|16G|
|/home|Linux home|Remainder of the device|

無論是`fdisk`還是`cfdisk`，用起來都很簡單，所以這裏不再具體介紹了，想要具體瞭解的可以查閱[fdisk官方文檔](https://wiki.archlinux.org/index.php/Fdisk)、[cfdisk官方文檔](https://wiki.archlinux.org/index.php/Cfdisk)。對於新人小白，個人推薦`cfdisk`，有一個僞圖形界面，很容易上手。

### 格式化分區
按照上面的步驟建立好分區之後，我們需要將每個分區用對應的文件系統進行格式化。

對於root分區、home分區等直接使用`ext4`文件系統進行初始化：
```bash
mkfs.ext4 /dev/sda2
mkfs.ext4 /dev/sda4
```

如果你的機器是UEFI啓動模式，使用以下命令初始化EFI系統分區：
```bash
mkfs.fat -F32 /dev/sda1
```

對於交換分區，不能使用上述命令進行格式化，而需要使用`mkswap`將其初始化：
```bash
mkswap /dev/sda3
swapon /dev/sda3
```

### 掛載分區

首先掛載`root`分區：
```bash
mount /dev/sda2 /mnt
```

對於其他分區（swap分區除外，不需要），需要自己手動創建掛載點：
```bash
mkdir /mnt/efi
mount /dev/sda1 /mnt/efi
mkdir /mnt/home
mount /dev/sda4 /mnt/home
```

### 選擇鏡像源
文件`/etc/pacman.d/mirrorlist`定义了软件包会从哪个镜像源下载。在 Live 启动的系统上，所有的镜像都被启用。在列表中越前的镜像在下载软件包时有越高的优先权。你可以相应的修改文件`/etc/pacman.d/mirrorlist`，并将地理位置最近的镜像源挪到文件的头部來保證下載速度。而且这个文件接下来还会被 pacstrap 拷贝到新系统里，在這裏設置好之後就可以一勞永逸了。

>這一步其實現在可以省略了，因爲現在在live環境中使用`reflector`進行鏡像的管理，貌似你一連接網絡，live系統會自動執行reflector命令來幫你選擇鏡像源，默認的是根據下載速率進行排序，感興趣的小夥伴可以自己確認一下（連接網絡之前備份一下`/etc/pacman.d/mirrorlist`文件，然後連接網絡之後，使用`diff`對比前後兩個文件的差異）。

### 安裝必需軟件包

使用archiso內置的腳本`pacstrap`安裝一些基本的軟件包、內核及常規的硬件固件：
```bash
pacstrap /mnt base linux linux-firmware
```

>這裏要注意的是，上面的命令並不包括所有的基本程序，如網絡管理程序、文本編輯器等，如果你想安裝這些程序，可以将名字添加到`pacstrap`后，并用空格隔开。你也可以在Chroot进新系统后使用`pacman`手动安装软件包或组。

到這一步，理論上其實新系統已經安裝完了，不過還無法正常使用:)所以需要進行配置以正常使用。

### 配置系統

#### 生成fstab文件
用以下命令生成`fstab`文件，其中`-U`選項用來設置UUID：
```
genfstab -U /mnt >> /mnt/etc/fstab
```

然後使用`cat /mnt/etc/fstab`命令檢查以下文件是否正確（每個分區佔一行）

### 進入到安裝的系統
```bash
arch-chroot /mnt
```

### 安裝文本編輯器
現在的新系統連默認的文本編輯器`nano`都沒有了，所以需要自己手動安裝一個，不然後面的一些配置無法實現，所以我選擇最強的`vim`：
```bash
pacman -S vim
```

### 時區
使用下面的命令設置時區：
```bash
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
```
然後使用`hwclock`生成`/etc/adjtime`文件：
```
hwclock --systohc
```

### 本地化設置
>本地化的程序与库若要本地化文本，都依赖[Locale](https://wiki.archlinux.org/index.php/Locale)，后者明确规定地域、货币、时区日期的格式、字符排列方式和其他本地化标准等等。在下面两个文件设置：`locale.gen`与`locale.conf`。

1. 首先編輯`/etc/locale.gen`文件，然後將需要的地區的註釋移除，建議將`en_US UTF-8`和`zh_CN UTF-8`都取消註釋。
2. 執行`locale-gen`命令生成locale。
3. 创建`locale.conf`文件并编辑`LANG`这一变量（将系统 locale 设置为`en_US.UTF-8`，系统的`Log`就会用英文显示，这样更容易问题的判断和处理。）：
```bash
LANG=en_US.UTF-8
```
*這裏最好不要設置爲中文locale，會導致TTY亂碼*

### 網絡設置

1. 創建`/etc/hostname`文件設置主機名
2. 配置`/etc/hosts`文件，將以下內容添加進去：
```
127.0.0.1	localhost
::1		localhost
127.0.1.1	myhostname.localdomain	myhostname
```
*note: 如果系统有一个永久的 IP 地址，请使用这个永久的 IP 地址而不是 127.0.1.1。*

### 設置root密碼

使用`passwd`命令設置root密碼即可。

這裏不得不說一句，使用arch是真的讓人舒服的一批，不會出現一些令人無語的問題，我還記得第一次使用linux的時候，用的是fedora，當時想要使用`su`進入root，但是發現需要密碼，但是我不記得安裝的時候設置過root密碼啊，後來查閱了一些資料才發現，fedora並沒有設置root密碼，需要自己手動設置（使用`sudo passwd root`）。

### 安裝及配置引導程序
>安裝引導程序之後才能進入系統

我用的引導程序是[GRUB](https://wiki.archlinux.org/index.php/GRUB)，首先安裝必要的軟件包：
```bash
pacman -S grub efibootmgr
```

這裏詳細介紹一下UEFI系統如何安裝配置GRUB：
1. 首先使用以下命令安裝到系統：
```bash
grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=ArchLinux
```
note: 因爲我的EFI分區在`/efi`目錄下，所以上述命令的`--efi-directory`參數就設置爲`/efi`
2. 使用`grub-mkconfig `生成grub配置文件：`grub-mkconfig -o /boot/grub/grub.cfg`

### 安裝wifi網絡管理工具
如果你不安裝的話，新系統是無法聯網的哦
我用的是`iwd`，簡單方便又強大
```bash
pacman -S iwd
```
### 重啓
1. 输入`exit`或按`Ctrl+d`退出`chroot`环境
2. 用`umount -R /mnt`手动卸载被挂载的分区
3. 执行`reboot`重启系统

## 再配置新系統
>因爲還沒有設置普通用戶，所以重啓後需要使用root來進入新系統

### 連接網絡
_如果你沒有安裝wifi網絡管理工具，或者你安裝的工具需要gui支持，那麼恭喜你，你的系統還是無法聯網，恭喜你可以重新體驗一次完整的archlinux安裝流程:)_

如果你按照我上面寫的安裝了wifi工具`iwd`，那麼你還需要做一些額外的工作才能正常使用它：
啓動iwd服務，爲了以後的使用，建議直接設置爲開機自啓動，詳細方法可以參考[systemd](https://wiki.archlinux.org/index.php/Systemd#Using_units)
```bash
systemctl start iwd.service  # 啓動服務
systemctl enable iwd.service # 開機自啓動服務
```

你是不是以爲這樣就能使用iwd來連接wifi了？那你就大錯特錯了， __你還需要啓動`systemd-networkd.service`和`systemd-resolved.service`才行，因爲新系統默認不會自啓動這兩個服務，需要你手動開啓__ (血的教訓換來的經驗，請珍惜)

到這裏你才能正常使用`iwd`這個工具，具體使用方法可以參考[前文](#網絡配置)

### 安裝顯示服務器
使用下面的命令安裝開源的xorg
```bash
pacman -S xorg
```
### 安裝顯卡驅動
根據自己的顯卡配置來選擇安裝即可。

我的機器配置爲intel集成顯卡+NVIDIA獨立顯卡：

對於intel顯卡，我安裝的是官方的`xf86-video-intel`驅動：
```bash
pacman -S xf86-video-intel
```

對於NVIDIA顯卡，我安裝的是開源驅動`nouveau`：
```bash
pacman -S mesa xf86-video-nouveau
```

### 安裝桌面環境
我使用的是`XFCE`，用起來不錯，之所以不想選擇KDE是因爲它太像windows了，而gnome感覺太臃腫了，所以最終選擇了XFCE，輕便又美觀。
使用以下命令安裝xfce：
```bash
pacman -S xfce4
```
安裝完成後還需要配置xinitrc文件來保證開機直接啓動

### 安裝顯示管理器
由於`xfce4`包里沒有顯示管理器(DM)，所以還需要自己選擇一個DM進行安裝，我選擇的是`lightdm`：
```bash
pacman -S lightdm lightdm-gtk-greeter
```

編輯`/etc/lightdm/lightdm.conf`設置`lightdm-gtk-greeter`爲默認的greeter：
```/etc/lightdm/lightdm.conf
[Seat:*]
...
greeter-session=lightdm-gtk-greeter
...
```

設置爲開機自啓動
```bash
systemctl enable lightdm.service
```

### 添加普通用戶
1. 使用以下命令添加一個用戶
```bash
useradd -m -G "附加组" -s "登陆shell" "用户名"
```
其中：
- `-m`/`--create-home`：创建用户主目录/home/[用户名]
- `-G`/`--groups`：用户要加入的附加组列表；使用逗号分隔多个组，不要添加空格；如果不设置，用户仅仅加入初始组。
- `-s`/`--shell`：用户默认登录shell的路径

2. 賦予用戶root權限
- 安裝sudo軟件包: `pacman -S sudo`
- 在`/etc/sudoers`文件中的`root ALL=(ALL) ALL`行下添加`yourname ALL=(ALL) ALL`

----------

至此，帶有圖形界面的arch系統安裝完成了，已經可以基本使用了，如果有其他需求，自己自行進行配置即可。

## 個性化系統

### 安裝中文輸入法
archlinux中文輸入法框架有`fcitx`/`ibus`/`fcitx5`，其中`fcitx5`貌似是剛剛出現的，之前沒見過，所以我就試了試，安裝配置起來還挺簡單的：
1. 安裝`fcitx5`軟件包
2. 創建`~/.pam_environment`並配置環境變量：
```~/.pam_environment
GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=@im=fcitx
```
3. 安裝中文輸入法引擎`fcitx5-rime`
4. 安裝輸入法模塊來保證輸入法在各種程序中使用：`pacman -S fcitx5-gtk fcitx5-qt5`
5. 啓動fcitx5，右鍵fcitx5的系統托盤圖標，點擊`configure`添加rime輸入法
6. `ctrl`+`space`鍵測試是否可以正常使用

### 安裝字體
```bash
pacman -S ttf-dejavu   # 英文字體
pacman -S wqy-microhei # 中文字體
```

### 安裝zsh
有一說一，默認的bash真的有點丑，所以安裝一個好看的shell很有必要，比如說zsh。安裝zsh的話，我推薦使用[oh my zsh](https://github.com/ohmyzsh/ohmyzsh)這個框架個性化zsh，配置zsh起來簡單方便。

1. 安裝zsh
2. 使用curl安裝ohmyzsh，這個腳本還可以幫你自動更改默認的shell
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

具體如何配置zsh，可以參考ohmyzsh的[官方wiki](https://github.com/ohmyzsh/ohmyzsh/wiki)。
