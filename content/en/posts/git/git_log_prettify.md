---
title: "How to prettify git log output"
date: 2019-07-10
description:
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- Git
- Tips
series:
- Git
categories:
- 2019-07
image:
---

### 4 formats
```bash
git log --color --date=format:'%Y-%m-%d %H:%M:%S' --pretty=format:'%C(cyan)%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cd) %C(bold blue)<%an>%Creset' --abbrev-commit  

git log --color --date=format:'%Y-%m-%d %H:%M:%S' --pretty=format:'%C(cyan)%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit  

git log --color --stat --date=format:'%Y-%m-%d %H:%M:%S' --pretty=format:'%C(cyan)%h%Creset -%C(yellow)%d%Cblue %s %Cgreen(%cd) %C(bold blue)<%an>%Creset' --abbrev-commit

git log --graph --date=format:'%Y-%m-%d %H:%M:%S' --pretty=format:'%C(cyan)%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cd) %C(bold blue)<%an>%Creset' --abbrev-commit  
```
### Config with alias
__For example:__

_use 4th format._

```bash
git config --global alias.logs "log --graph --date=format:'%Y-%m-%d %H:%M:%S' --pretty=format:'%C(cyan)%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cd) %C(bold blue)<%an>%Creset' --abbrev-commit"
```