---
title: "Use Systemd to Start a Linux Service at Boot"
date: 2020-06-29T18:17:20+08:00
description:
url: /zh/tips/autostart_a_service_at_boot_on_linux
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- Linux
- Shaodowsocks
- Terminal
- Tips
series:
-
categories:
- 2020-06
image:
---

## 创建自定义systemd Service

1. 创建一个脚本或者使用可执行文件，本文以一个`test.bash`脚本为例：
```bash
DATE=`date '+%Y-%m-%d %H:%M:%S'`
echo "Example service started at ${DATE}"
while :
do
echo "...";
sleep 1000;
done
```

2. 使该脚本具有可执行权限：
```bash
sudo chmod +x /usr/bin/test.sh
```

3. 创建一个名为`testservice.service`的`Unit file`来定义一个`systemd`服务：

```bash
[Unit]
Description=Example systemd service.

[Service]
Type=simple
ExecStart=/usr/bin/zsh ~/.local/bin/test.sh

[Install]
WantedBy=multi-user.target
```

4. 将上述`Unit`文件复制到/etc/systemd/system并为其授予权限：
```
sudo cp testservice.service /etc/systemd/system/testservice.service
sudo chmod 644 /etc/systemd/system/testservice.service
```

>想要具体了解Unit文件的可用配置参数，可以查阅[systemd](https://www.freedesktop.org/wiki/Software/systemd/)

## 启动服务
在命令行输入以下命令启动服务：
```bash
sudo systemctl start testservice
```

使用以下enable命令来确保该服务在系统启动时启动：
```bash
sudo systemctl enable testservice
```

如果想要检查服务状态，使用：
```
sudo systemctl status testservice
```

你也可以使用以下systemd命令停止或重新启动该服务：
```bash
sudo systemctl stop testservice
sudo systemctl restart testservice
```


