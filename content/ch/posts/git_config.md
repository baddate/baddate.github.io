---
title: Git config tips
date: 2019-07-10
lastmod: 2019-11-14 
weight: 4
url:
    /git/config
tags:
    - Tips  
    - Git
    - Proxy
    - SSH
categories:
    - 2019-07
description: "some tips about git"

---


## User name

1. Open Terminal.

2. Set a Git username:
`git config --global user.name "yourname"`

3. Confirm that you have set the Git username correctly:

```bash
$ git config --global user.name
> yourname
```

## User email

1. Open Terminal.

2. Set a Git useremail:
`git config --global user.email "email@addr.com"`

3. Confirm that you have set the Git user email correctly:

```bash
$ git config --global user.email
> email@addr.com
```

## Add ssh key
### generate ssh key
1. Open git bash and run `ssh-keygen -t rsa -C "youremail"`
2. Open github settings and add `rsa.pub` file to ssh key

## Set proxy

### set a socks5 proxy

```bash
git config --global http.proxy socks5://127.0.0.1:1080
git config --global https.proxy socks5://127.0.0.1:1080
```

### set a http proxy

```bash
git config --global http.proxy http://127.0.0.1:1080
git config --global https.proxy http://127.0.0.1:1080
```

### unset proxy

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

## Git_synctips

### The remote repository has changed and the local repository has not changed

>1. git remote -v
>2. git fetch origin master
>3. git log master.. origin/master
>4. git merge origin/master

### The remote repository has not changed and the local repository has changed

>1. git add -A or git add . or git add --all
>2. git commit -m "the content of your modify"
>3. git push -u origin master
