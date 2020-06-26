---
title: "How to sync git repo"
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

### The remote repository has changed and the local repository has not changed

>1. git remote -v
>2. git fetch origin master
>3. git log master.. origin/master
>4. git merge origin/master

### The remote repository has not changed and the local repository has changed

>1. git add -A or git add . or git add --all
>2. git commit -m "the content of your modify"
>3. git push -u origin master
