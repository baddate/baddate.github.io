---
title: "如何将Julia添加到Jupyter Notebook"
date: 2020-06-29T10:34:22+08:00
description: "How to Add Julia to Jupyter Notebook"
url:
    /zh/tips/add_julia_to_jupyter
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- Tips
- Julia
- JupyterNotebook
series:
-
categories:
- 2020-06
image:
---

>[Julia](https://julialang.org/)是一门灵活的动态语言,适合用于科学计算和数值计算,并且性能可与传统的静态类型语言媲美。[JupyterNotebook](http://jupyter.org/)是目前最流行的数据科学Web程序，功能涵盖数据清理和转换，数值模拟，统计建模，数据可视化，机器学习等。本文旨在使用[IJulia](https://github.com/JuliaLang/IJulia.jl)将Julia集成到Jupyter交互式环境中。本文假设你已经安装好了Julia和Jupyter环境。

1. 在命令行键入`julia`，运行Julia程序，出现以下提示:
```bash
$ julia
   _       _ _(_)_     |  Documentation: https://docs.julialang.org
  (_)     | (_) (_)    |
   _ _   _| |_  __ _   |  Type "?" for help, "]?" for Pkg help.
  | | | | | | |/ _` |  |
  | | |_| | | | (_| |  |  Version 1.4.2 (2020-05-23)
 _/ |\__'_|_|_|\__'_|  |  Official https://julialang.org/ release
|__/                   |

julia>
```

2. 在提示符`julia>`后键入以下内容安装`IJulia`：
```julia
using Pkg
Pkg.add("IJulia")
```

3. 安装完成后，在提示符中键入以下内容启动带有Julia环境的Jupyter Notebook：
```julia
using IJulia
notebook()
```
*p.s.* 如果你想使用最新的Jupyter Lab，可以使用`jupyterlab()`命令替换上面的`notebook()`命令。

4. 上面的`notebook()`命令会启动Jupyter Notebook，然后点击`New`选择New文件的类型为`Julia 1.4.2`，即可编写`julia`代码：
![new julia notebook](/img/new_julia_notebook.png)
例如，使用`println`函数打印`hello, julia!`：
![julia println](/img/use_julia_on_jupyter.png)