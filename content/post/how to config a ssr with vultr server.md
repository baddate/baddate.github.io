---
title: "How to config a VPN with vultr(一键脚本搭建SS/搭建SSR服务并开启BBR加速)"
date: 2018-10-01
lastmod: 2018-10-01 22:48:45
url:
    /tips/vpn_config
tags:
    - Tips
    - VPN
    - ShadowsocksR
categories:
    - 2018-10
---

## 购买服务器
**在这里使用的是vultr服务器**
1. 到[这里](https://www.vultr.com/)注册一个账户.
2. 充值账户(至少10$，第一次充值只支持PayPal和银行卡，但之后再充值可以使用支付宝和微信), 使用优惠码(https://www.vultr.com/?ref=7407589)会送25$.
3. 购买一个服务器，目前最便宜的是2.5刀一个月，但是不支持ipv4，所以建议买3.5刀的，ipv4/ipv6都支持.*操作系统选择使用centos，这样方便接下来的脚本搭建*

## 连接服务器

推荐使用[MobaXterm](https://mobaxterm.mobatek.net/)免费而且很强大，或者直接在我分享的链接里下载: 
*链接: https://pan.baidu.com/s/1G4pf6sypXkAvhbAqHpZtdQ 提取码: xnvx*

！[MobaXterm的主界面](/static/vpn/MobaXterm_mainUI.png "MobaXterm的主界面")
1. 在下图所示界面输入你购买的服务器IP
![ip](/static/vpn/SSH_UI.png "IP输入界面")

2. 点击确定之后，在这里输入你的用户名（默认是root）和密码（vultr自动生成的字符串，你在里面输入时不显示密码的，所以最好直接copy，粘贴的时候不要用快捷键，直接右键点击paste）
![输入用户名/密码](/static/vpn/user.png "输入用户名/密码")
3. 如果输入正确，有如下界面，则登陆成功，接下来就是配置了
![服务器界面](/static/vpn/login.png "服务器界面")

## 配置SSR

1. 下载一键搭建ss脚本文件     
`yum -y install git && git clone https://github.com/Flyzy2005/ss-fly`

2. 运行搭建ssr脚本代码
`ss-fly/ss-fly.sh -ssr`
3. 这之后会进入到输入参数的界面，设置密码，端口等参数

4. 相关SSR命令
```bash
启动：/etc/init.d/shadowsocks start
停止：/etc/init.d/shadowsocks stop
重启：/etc/init.d/shadowsocks restart
状态：/etc/init.d/shadowsocks status
 
配置文件路径：/etc/shadowsocks.json
日志文件路径：/var/log/shadowsocks.log
代码安装目录：/usr/local/shadowsocks
```
5. 卸载SSR服务
`./shadowsocksR.sh uninstall`


## 开启BBR加速
*执行命令*      
`ss-fly/ss-fly.sh -bbr`

## 参考
https://www.flyzy2005.cn/fan-qiang/shadowsocks/install-shadowsocks-in-one-command/#ss

**THE END**     
