---
title: find 'New' command
draft: true
date: 
    2018-08-12 22:20:20
lastmod: 2019-01-10 17:37:00
tags:
    - Tips
    - Windows
url:
    /en/tips/windows_tips
categories:
    - 2018-08
---

> 有的时候我们会发现自己常用的“新建”文件类型丢失了，这时候我们可以修改注册表来添加找回。

**假设丢失的是新建`.txt` 文件类型**     
1. 用记事本新建一个文本文件，命名为“ADDTXT.REG”     
2. 输入以下代码：      
`Windows Registry Editor Version 5.00`          
`[HKEY_CLASSES_ROOT\.TXT\ShellNew]"NullFile"=""`        
3. 保存该文件并执行。        

>REFERENCE： 
电脑爱好者2018-12
