---
title: "How to Create Linux Desktop Entry"
date: 2020-06-26T10:11:55+08:00
description:
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
    - Tips
    - Linux
series:
    - 
categories:
    - 2020-06
image:
---

1. create a file named `yourappname.desktop` in `/usr/share/applications/` directory or `~/.local/share/applications/`.
2. input the following lines:
```bash
[Desktop Entry]
Type=Application
Terminal=false
Exec=/path/to/app
Name=app
Comment=app comment
Icon=/path/to/app/icon
```