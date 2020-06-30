---
title: "A List of Terminal Tools"
date: 2020-03-27T13:05:26+08:00
description: "自己使用的一些好用的命令行工具"
url:
    /zh/tools/a_list_of_terminal_tools
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- Git
- Terminal
- Tools
series:
-
categories:
- 2020-03
image:
---

## ohmyzsh
[ohmyzsh](https://github.com/ohmyzsh/ohmyzsh)一个开源的、社区驱动的框架，用于管理[zsh](https://www.zsh.org/)配置。可以简单地配置出一个美观又实用的shell。
![ohmyzsh example](/img/ohmyzsh_example.png)
## ag - The Silver Searcher
一款类似于`ack`的代码搜索工具，但是比`ack`要快很多（一个数量级）。更神奇的一点是，它会忽略与`.gitignore` and `.hgignore`中匹配的文件。

安装也很简单，支持大部分主流的操作系统，例如，我使用的是`fedora`，安装命令为：`sudo dnf install the_silver_searcher`。有的人可能会奇怪为什么安装包的名字不是`ag`，关于这一点，其实我也很奇怪:)

关于具体的特点及使用可以查阅它的[开源仓库](https://github.com/ggreer/the_silver_searcher)。
## tldr
[tldr](https://github.com/tldr-pages/tldr)是一个简化版的`man pages`。如果你的英语水平不太好或者有时候只是想简单了解某个命令，那么就使用tldr吧。

![tldr](/img/tldr_screenshot.png)
## fzf
[fzf](https://github.com/junegunn/fzf)是一个交互式Unix命令行过滤器，可以与任何列表一起使用。文件，命令历史记录，进程，主机名，书签，git commit等。

更厉害的是，它还可以集成到vim或者zsh/bash中，具体步骤可以参阅[官方文档](https://github.com/junegunn/fzf/blob/master/README.md)。

![fzf example](/img/fzf_preview.png)
## ncdu
[Ncdu](https://dev.yorhel.nl/ncdu)是具有ncurses接口的磁盘使用情况分析器。可以简单快速地查看磁盘的使用情况。

![ncdu screenshot](/img/ncdu_screenshot.png)
## htop
[htop](https://github.com/hishamhm/htop)是用于Unix系统的交互式文本模式进程查看器。比`top`更美观、更方便。
![Htop example](/img/htop_screenshot.png)
## ShellCheck
[ShellCheck](https://github.com/koalaman/shellcheck)是一款用于Shell脚本的静态分析工具。可以检查脚本的语法问题并提供建议。
![shellcheck example](/img/shellcheck_example.png)
## tig
[tig](https://github.com/jonas/tig)是git的基于ncurses的文本模式界面。主要用于浏览代码仓库，也可以当作git来使用，不过值得一提的是，`tig`的命令输出样式更为美观。你可以把它当作一个git的美化版。

下图展示了tig浏览代码仓库时，回车查看commit的diff细节，具体的操作命令可以按`h`来获取其他快捷键：
![tig screenshot](/img/tig_screenshot.png)

暂时就介绍这么多了，之后碰到好用的会再补充。