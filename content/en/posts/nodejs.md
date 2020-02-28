---
title: nodejs10.x on CentOS7.6
date: 2019-09-23
lastmod: 2019-09-23 
weight: 1
url:
    /nodejs
tags:
    - Tips  
    - Nodejs
    - Yarn
    - NPM
categories:
    - 2019-09
description: "some tips about nodejs"

---

## install nodejs10.x

```bash
curl -sL https://rpm.nodesource.com/setup_10.x | bash -
sudo yum install nodejs
```

## install yarn

```bash
curl -sL https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
yum install yarn
```

## npm change src mirror

### 更换为淘宝源

```bash
# temp use
npm --registry https://registry.npm.taobao.org install express
# change permanently
npm config set registry https://registry.npm.taobao.org
# check
npm config get registry
```
