---
title: Algorithm Problems on POJ
tags: 
    - Algorithm
date: 2018-06-07 
url: /solutions/Algorithm
categories:
    - SOLUTIONS
---
*算法题解*
<!-- more --> 
# Divide and Conquer Algorithm
## Raid
__[from ID3714](http://poj.org/problem?id=3714)__  

  
最近点对问题(Closest Pair)的进阶版, 增加一个限制, 即判断两个点是不是来自同一阵营   
*思路*  
利用分治思想, 划分为两部分pl和pr, 两点之间计算距离时先判断是不是在同一阵营, 如果在同一阵营, 返回一个极大值(+oo), 其他和Closest Pair问题解法大同小异

__代码:__     
```c++

```
_Reference:_    
<https://www.geeksforgeeks.org/closest-pair-of-points/>     
<http://www.cnblogs.com/oyking/p/3197074.html>

# Dynamic Programming
1458 
1050 
# Greedy Algorithm
2393 1328   
