---
title: "Git_synctips"
date: 2018-08-14T12:49:10+08:00
lastmod: 2018-08-14T12:49:10+08:00
tags: 
    - Tips
    - Git
categories: 
    - TIPS
url: 
    /tips/git_synctips
draft: false
---

## The remote repository has changed and the local repository has not changed.
>1. git remote -v 
>2. git fetch origin master
>3. git log master.. origin/master 
>4. git merge origin/master

## The remote repository has not changed and the local repository has changed.
>1. git add -A or git add . or git add --all
>2. git commit -m "the content of your modify"
>3. git push -u origin master