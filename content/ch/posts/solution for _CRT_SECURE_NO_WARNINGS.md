---
title: Solution for _CRT_SECURE_NO_WARNINGS 
date: 2019-07-10
lastmod: 2019-07-10 
weight: 5
url:
    /solution/windows_vs2019
tags:
    - Solution  
    - Windows
    - VS2019
    - C++
categories:
    - 2019-07

---

> platform: VS2019	
> os: win10

Problem:   
An error about `_CRT_SECURE_NO_WARNINGS` when I use the `std::localtime()` function for getting the current systime.

Solution:   
1. Use an alternative feature function in C++.	
2. add this line at the first line.	
```
#define _CRT_SECURE_NO_WARNINGS
```
3. add `_CRT_SECURE_NO_WARNINGS` on the pre-processor(project>properties>C/C++>preprocessor)

