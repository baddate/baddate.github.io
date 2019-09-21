---
title: "JavaWeb 踩坑记"
date: 2019-08-24
lastmod: 2019-08-24
tags: 
    - Solutions
    - Java
    - Servlet
    - JSP
    - JSTL
    - MySQL
categories: 
    - 2019-09
url: 
    /solutions/javaweb
draft: true

description:
---

## 关于 "java.sql.SQLException: Before start of result set" 异常

```
原因：  
    ResultSet对象代表SQL语句执行的结果集，维护指向其当前数据行的光标。每调用一次next()方法，光标向下移动一行。最初它位于第一行之前，因此第一次调用next()应把光标置于第一行上，使它成为当前行。
解决办法:       
    使用`rs.getString();`前加上`rs.next();`即可
```
## JSP中的字符比较
举个例子说明：          

*注意其中要比较的字符串需要用**单引号**引起来，使用双引号会报错*

```jsp
<c:if test="${user.role == 'superadmin'}">
```


