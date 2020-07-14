---
title: "Linux挂起和休眠的区别"
date: 2020-07-07T11:00:20+08:00
description:
url: /zh/linux_suspend_vs_hibernate
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- Tips
- Linux
- ArchLinux
- System
series:
-
categories:
- 2020-07
image:
---

linux的挂起和休眠之间的区别到底是什么？

Linux有三种挂起方式：挂起至RAM（通常称为suspend），挂起至磁盘（通常称为hibernate）和混合挂起（hybrid suspend）

## 挂起至RAM(Suspend，译为睡眠)
该模式通过将系统状态保存在RAM 中使计算机进入睡眠状态。在这种状态下，计算机进入低功耗模式，但是系统仍然需要电源才能将数据保留在RAM中。

>需要明确的是，挂起不会关闭计算机。

## 挂起至磁盘(Hibernate，译为休眠)
该模式将内存的当前内容移入[SWAP]空间。在这种状态下，计算机不需要电源。当你启动计算机时，所有内容都会复制回RAM，然后从上次中断的地方继续。

>需要明确的是，计算机已完全关闭了Hibernate的电源。

## 混合挂起(Hybrid Suspend)
该模式结合了以上两种模式的特点，将计算机的状态保存到交换空间中，但不关闭计算机电源，而是是调用suspend模式。因此，如果电池没有耗尽，则系统可以从RAM恢复。如果电池电量耗尽，则可以从磁盘恢复系统，这比从RAM恢复速度要慢得多，但是计算机的状态并未丢失。

--------

对于ArchLinux，默认的管理接口是`systemd`，如果你没有安装其他的电源管理软件的话，你需要手动设置才可以正常使用休眠功能。当然，前提是你有设置`SWAP`分区。

# 设置休眠模式


