---
title: "Install and Use Shadowsocks Cmd Client on Linux"
date: 2020-06-27T21:46:36+08:00
description: "在Linux上使用shadowsocks的命令行客户端"
url:
    /zh/tips/install_shadowsocks_cmd_client_on_linux
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- Tips
- Proxy
- Terminal
- Linux
- Tools
- Socks5
series:
-
categories:
- 2020-06
image: /img/shadowsocks_gfw.jpeg
---

最近在[Fedora](https://fedoraproject.org/)上安装shadowsocks-qt5一直提示依赖库缺失，最后也没找到解决办法，所以就想着安装命令行客户端来应急使用，本文记录了一下自己的安装经验。

## 安装命令行客户端

~~shadowsocks没有收录在fedora的软件包中，所以需要使用`pip`来安装它：`sudo yum install python-setuptools` or `sudo dnf install python-setuptools` and `sudo easy_install pipsudo pip install shadowsocks`~~

shadowsocks已经收录在fedora的软件包中，直接使用dnf来安装即可：
```bash
sudo dnf install python3-shadowsocks
```

## 配置shadowsocks
### Step 1
在`/etc`目录下创建配置文件`shadowsocks.json`：
```bash
sudo vim /etc/shadowsocks.json
```
### Step 2
将以下文本放入文件中。用实际IP替换server-ip并设置密码：
```json
{
"server":"server-ip",
"server_port":8000,
"local_address": "127.0.0.1",
"local_port":1080,
"password":"your-password",
"timeout":600,
"method":"aes-256-cfb"
}
```
### Step 3
在命令行启动shadowsocks客户端，参数的具体含义可以使用`sslocal --help`获取：
```bash
sslocal -c /etc/shadowsocks.json
```

如果想要在后台运行，需要加入`-d start`参数：
```bash
sudo sslocal -c /etc/shadowsocks.json -d start
```

到此，shadowsocks已经配置完成，可以正常使用了。但是有一点麻烦的是，需要每次在命令行中手动启动它，所以配置成开机自启动我觉得很有必要。具体操作可以参考我写的另一篇[文章](/zh/tips/autostart_a_service_at_boot_on_linux)。