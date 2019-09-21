---
title: Git config username & user-email and proxy
date: 2019-07-10
lastmod: 2019-09-21 
weight: 4
url:
    /git/config
tags:
    - Tips  
    - Git
categories:
    - 2019-07
description: "some tips qabout git"
---


## User name
1. Open Terminal.

2. Set a Git username:

```bash
$ git config --global user.name "yourname"
```

3. Confirm that you have set the Git username correctly:

```bash
$ git config --global user.name
> yourname
```

## User email
1. Open Terminal.

2. Set a Git useremail:

```bash
$ git config --global user.email "email@addr.com"
```

3. Confirm that you have set the Git user email correctly:

```bash
$ git config --global user.email
> email@addr.com
```

## Set proxy

### set a socks5 proxy
```
git config --global http.proxy socks5://127.0.0.1:1080
git config --global https.proxy socks5://127.0.0.1:1080
```

### set a http proxy
```
git config --global http.proxy http://127.0.0.1:1080
git config --global https.proxy http://127.0.0.1:1080
```

### unset proxy
```
git config --global --unset http.proxy
git config --global --unset https.proxy
```