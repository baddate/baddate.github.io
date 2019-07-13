---
title: NOHUP command details 
date: 2019-07-10
lastmod: 2019-07-10 
weight: 2
url:
    /linux/nohup
tags:
    - Tips  
    - Linux
    - CMD
categories:
    - 2019-07
---


 **操作系统中有三个常用的流：**

> 0：标准输入流 stdin	    
> 1：标准输出流 stdout	
> 2：标准错误流 stderr	
> 一般当我们用 > console.txt，实际是 1>console.txt的省略用法；< console.txt ，实际是 0 < console.txt的省略用法。	
  
例子：   
```bash
nohup ./start-dishi.sh >output 2>&1 &
```

解释：	

 1. 带&的命令行，即使terminal（终端）关闭，或者电脑死机程序依然运行（前提是你把程序递交到服务器上)； 
 2. 2>&1的意思是把标准错误（2）重定向到标准输出中（1），而标准输出又导入文件output里面，所以结果是标准错误和标准输出都导入文件output里面了。 至于为什么需要将标准错误重定向到标准输出的原因，那就归结为标准错误没有缓冲区，而stdout有。这就会导致 >output 2>output 文件output被两次打开，而stdout和stderr将会竞争覆盖，这肯定不是我门想要的. 这就是为什么有人会写成： nohup ./command.sh >output 2>output出错的原因了
