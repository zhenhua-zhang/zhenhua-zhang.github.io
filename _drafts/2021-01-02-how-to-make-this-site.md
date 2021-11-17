---
title: 构建本网站的流程
date: 2021-01-02 10:05:47
tags: [misc]
categories: [Misc]
---

## 简介

本站点使用非常受欢迎的博客生成框架`Hexo`构建。
该框架使用大名鼎鼎的`Node.js`来完成页面渲染，速度非常快，号称几秒内生成上百个网页。
支持`Markdown`，让发表博文变得非常简单，关键是`Markdown`已经足够满足我对写作的需求。
能非常方便的一键部署，前期配置稍微繁琐，但换来的是快速部署，当然预览也是非常方便的。
丰富的插件和强大的扩展性，一方面让博客部署变得非常简单，另一方面可以让页面变得非常绚丽。

## 目标
前面简单介绍了`Hexo`，但本文的重点是实现以下三个目标。

- 使用`Hexo`生成静态站点，并配置一键部署、使用自定义主题等。
- 使用`GitHub Pages`功能托管一个博客站点。
- 使用自己的域名来访问前面生成的站点。

其中第三个目标是可选的，毕竟不是每个人都需要一个自己的网址，`GitHub`给的免费网址已经够用了。
如果自己的`GitHub`名称很具有识别性，那就更好了。

<!--more-->

## 准备工作

### 首先需要安装Hexo
`Hexo`的有两个重要依赖：`Git`和`Node.js`。
如果已有前面的两个程序，而且能正常使用，可以跳过下面的步骤，直接去看xxx的内容。
我平时用`Debian`，所以下面都是再`Linux`平台下的操作。
需要其它平台的，可前往官方文档查阅。官方文档也有中文版，但感觉不够详细。

#### 安装`Git`
我自己的电脑我有`root`权限，所以直接用`sudo`来安装。

```
sudo apt install git-core
```

当然，还有其它的方式，比如下载特定发行版本的`Git`自行编译后部署，但显然此处不涉及这方面的内容。
自行编译并部署的方法是为了获取特定版本的程序，这对于使用`Debian`的人来说是家常便饭，毕竟该发行版本非常注重程序的稳定性（这也是我选用`Debian`的重要原因）。

#### 安装`Node.js`
同样的，使用`sudo`来安装。

```
sudo apt install nodejs
```

#### 安装`Hexo`
其实`Hexo`是用`JavaScript`写成的一个包，该包被托管再[npm](https://www.npmjs.com)，在前面成功安装`Node.js`后，可以很方便的使用一并安装的`npm`命令来安装需要的包。所以安装`Hexo`简单到只需要一个命令行。


```
npm install -g hexo-cli
```

### 在`GitHub`上创建一个代码仓库用来存储`GitHub Pages`
我们的最终目的是用`GitHub`的`GitHub Pages`功能来托管我们的站点。
所以需要根据创建`GitHub Pages`的方法来创建一个用于托管网站内容的仓库。
该部分内容在`GitHub`上有很详细的步骤说明，在此略过该部分。
但为了后面用`hexo`命令创建新的网站，假定我们在`GitHub`上有一个名为`github-account-name.github.io`的仓库。
为了在提交内容时不产生冲突，我们不在该仓库中添加任何文件。

### 创建一个网站的基本框架
我们在完成前面的步骤后，后续的工作便是新建一个网站的基本框架。
`Hexo`会提供一个默认的主题，但是该主题比较朴素，所以我们后续会使用一个非常漂亮的主题，名叫[NexT](https://theme-next.org)。

第一步是用`hexo`起始一个新的项目，我们暂时把这个项目命名为`github-account-name.github.io`。
```
$ hexo init github-account-name.github.io
```

然后需要进入到该文件中并安装必要的包。
```
$ cd github-account-name.github.io
$ npm install
```

新建后的文件夹下应该至少有一下内容。不同版本的`Hexo`构建的网站基本结构可能存在差别，但`_config.yml`是必须的。
```
github-account-name.github.io
├── _config.landscape.yml
├── _config.yml
├── node_modules
├── package.json
├── package-lock.json
├── scaffolds
├── source
└── themes
```

<!-- vim: set ai nospell ts=4 tw=500 ft=pandoc:-->
