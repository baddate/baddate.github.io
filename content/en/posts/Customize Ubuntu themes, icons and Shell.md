---
title: Customize Ubuntu themes, icons and Shell
date: 2018-12-07
lastmod: 2018-12-11 22:38:58
url:
tags:
    - Tips  
    - Ubuntu
categories:
    - 2018-12

---

## Pre-requirements
1. gtk+3 [download](https://www.gtk.org/download/linux.php)	
2. gnome-tweak-tool
	`sudo apt-get update` and `sudo apt-get gnome-tweak-tool`
![Tweaks](/img/tweak-screen.png)
3. Install an gnome shell Extension
`sudo apt-get install gnome-shell-extensions`
**Reboot after executing this command**
Then you can find a extensions named *user themes*
Make it `on`
![extensions](/img/tweak-extensions.png)

## Start!
### Install an GTK+ themes
1. Download themes files from [here](https://www.opendesktop.org/s/Gnome/p/1241688).
![test](/img/macthemes.png)
2. Decompression files use these commands
`xz -d filename.tar.xz`
`tar xvf filename.tar`
then you can see a folder on current directiry
3. Move the folder to *themes* folder
`sudo mv filename /usr/share/themes`
4. Open Tweaks and change themes on *Appearence->Themes->Applications*
![them-done](/img/themes-done.png)

### Install macOS ICON
1. Download file from [here](https://www.opendesktop.org/s/Gnome/p/1102582/)
similar to install themes
2. Decompression files and move it to */home/baddate/.icons*
**maybe you should press ctrl + h to show hidden file, if it doesn't exists, you can new one folder named** `.icon` **then move the icons foder**
3. Go to Tweaks `Appearence` window and select `icon`.

### Install mac similar Shell
1. Download file from [here](https://www.opendesktop.org/s/Gnome/p/1013741/)
2. Select `Sierra-compact-light.tar.xz` or`Sierra-compact-dark.tar.xz`.
3. Decompression it and move it to /usr/share/themes.
4. Go to Tweaks `Appearence` window and select `Shell`.

That's all.
Enjoy your Ubuntu-Mac!
