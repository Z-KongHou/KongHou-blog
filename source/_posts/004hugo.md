---
title: How to build a Hugo site
tag: [Hugo]
categories: [课业学习]
---

# Start a Hugo site

## Discrimination

这是一篇关于笔者是如何使用 **Hugo** 来搭一个基本网站的 blog

## Content

### Install

在开始用 **hugo** 搭建网站之前,请先安装对应系统的二进制文件,为方便使用，将其 `bin` 文件夹添加到自己的环境变量中是个不错的选择

安装完成后命令行运行：

```bash
$ hugo version
```

若能看到安装结果则为成功

### Quick Start

下面为项目起步示例：

```bash
$ hugo new site <project> #新建一个hugo目录
$ cd <project> #进入目录
$ git init #初始化Git仓库
$ git submodule add <theme> #添加对应主题
$ echo "theme = <theme>" >> hugo.toml #在配置文件中指定当前使用的主题
$ hugo server #启动hugo服务
```

按下 `ctrl` + `c` 终止 hugo 服务

### Edit Sth

当想在项目中添加新文章时，可以在项目根目录下运行

```bash
$ hugo new content/posts/my-first-post.md
```

此时，在编辑器打开 `my-first-post.md` 即可编写对应内容

### Set Config

如若想要配置站点布局等内容，需在对应主题文档上查看配置操作，在此不再多述

## Ending

Congratulations！你已成功掌握如何起步一个基于 **hugo** 的网站！！！
