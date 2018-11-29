---
title: Set SSR on ubuntu18.04
date: 2018-11-28
lastmod: 2018-11-30 14:23:09
url:
tags:
    - Tips  
categories:
    - 2018-11
---
- Install a client from [HERE](https://github.com/erguottou520/electron-ssr), It is an amazing ssr GUI!
- Open it and setting params.
- Setting Firefox browser. 
*One way*
    1. `Preferences` -> `General` -> `Network Settings` -> `Manual Proxy configuration`
    2. `SOCKS Host:127.0.0.1` and `Port:1080`.ps:use yours port if you set another port.
    3. Clicking `OK`.
    4. Enjoying!
     

*Another way*
    1. Adding this Add-on called [FoxyProxy Standard](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/).
    2. Setting this Add-on by clicking `Options`, it will pop a tab. 
    3. Enter`Edit`button then setting IP address(127.0.0.1) and Port(1080).
    4. Enjoying!
