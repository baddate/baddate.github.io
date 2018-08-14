---
title: "Hosting on GitHub with Hugo(Windows)"
date: 2018-07-25T20:14:36+08:00
tags:
    - Tips
url:
    /tips/github_tips
categories:
    - TIPS
---


## 安装Git

1. 在[这个页面](https://git-scm.com/downloads)下载最新版的Git。
2. 然后按照wizard指引安装Git。
3. 配置环境变量
```mermaid
graph LR
a[control panel] --> b[System and Security]
b[System and Security] --> c[System]
c[System] --> d[Advanced system settings]
d[Advanced system settings] --> e[ENvirment Variables..]
e[ENvirment Variables..] --> f[select Path then Edit]
f[select Path then Edit] --> g[add git path]
g[add git path] --> Done!
```

## 创建GitHub账号
**这个自行[百度](http://www.baidu.com/) or [Google](https://www.google.com/)**        

## 安装Hugo
1. 从[Hugo Releases](https://github.com/gohugoio/hugo/releases)下载Hugo压缩包(记得注意系统位数是32 or 64)。
2. 解压压缩包
3. 配置环境变量(同上)

当然还有其他方法，这边就暂时不介绍了，有兴趣的同学可以参考[这里](https://gohugo.io/getting-started/installing/).

## 配置Hugo
### 创建网站
使用`hugo new site sitename`命令来创建一个网站模板
模板包含下面这些文件
    
    ├── archetypes
    ├── content
    ├── data
    ├── layouts
    ├── static
    ├── themes
    └── config.toml


### 安装主题
1. 在这个[Hugo 主题](https://themes.gohugo.io/)页面找一个喜欢的主题下载并安装。
2. 下载的应该是一个压缩包，把它解压到`themes`这个文件夹里面。
3. 接下来就是配置网站了，在上面我们可以发现有一个名为`config.toml`的文件，所以我们只要编辑这个文件就可以配置我们的网站了。<br>
我们上面只是下载解压了那个主题文件夹，并没有把它加载到我们的创建的网站上，所以这个时候我们只要在`config.toml`这个文件里添加一行`themes = "theme_name"`就搞定了。<br>
主题也安装好了，接下来就可以试试效果了：        

1. 我们在`content`文件夹里面新建一个文件夹，命名为`post`(一定要是这个名字哦，这个是默认的，如果要用其他名字，还需要在**config*文件里面添加一行代码，这里暂时不做讨论了)
2. 在网站根目录(即: ./sitename下)打开命令行(`win`+`R`,然后输入`cmd`), 输入`hugo server -t theme_name`
3. 我们打开(http://localhost:1313)就可以看到了！这个时候我们会发现多了一个`public`文件夹，这个文件夹包含了我们生成的网页内容
当然，这个时候才刚刚完成了一半，因为现在只有我们自己可以看到这个网页，别人是看不到的。所以，这个时候我们就要把它部署到服务器上了，服务器可以有好多选择，这里就只讨论Github pages了，毕竟是免费还不限流量的嘛！

## 在Github Pages上部署

### 创建 GitHub pages

这个可以参考[官方教程](https://pages.github.com/)，就不赘述了，有疑问的同学可以私信我。

### 在Github上托管
1. 在 GitHub 上创建 `your-project-name` 代码仓库（它将用于托管 Hugo 的内容）
2. 在 GitHub 上创建 `username.github.io` 代码仓库（它将用于托管 `public` 文件夹：静态的网站）
3. git clone <your-project-name-url> && cd <your-project-name>
4. 使用`hugo new site sitename`来创建网站，在<your-project-name>目录下。
5. 让你的网站在本地生效，输入`hugo server -t theme_name`
6. 看到效果后，我们这时候需要把`public`这个文件夹删除了
7. git submodule add git@github.com:<username>/<username>.github.io.git public (这里又生成一个`public`文件夹，这就是我们的网页文件)
8. 最后，我们添加一个脚本(deploy.sh)来帮我们简化操作


```bash
#!/bin/bash

echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

# Build the project.
hugo # if using a theme, replace by `hugo -t <yourtheme>`

# Go To Public folder
cd public
# Add changes to git.
git add -A

# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin master

# Come Back
cd ..
```


**到这里我们的个人网站就建成了！！！**让我们马上看看效果吧(http://username.github.io/), 可能会有些延迟哦，要有耐心。 

## END 