---
title: "How to config Git proxy"
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
- Proxy
- Socks
series:
- Git
categories:
- 2019-07
image:
---

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