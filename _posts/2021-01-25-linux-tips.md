---
title: 命令行实例（Linux）
date: 2021-01-25
tags: [linux,cli]
categories: [Linux]
---

## 简介
我平时的工作环境是Debian，用的最多的工具是终端，更确切地说是Neovim内置的终端模拟器。
命令行工具是我最喜欢的工具类型，方便快捷，节省资源。
当然我没有说图形化的工具不好，好看好玩儿的图形化工具我也很喜欢，只是觉得命令行更简洁。
再就是生信行业里工具更新换代快，针对的用户群体也特殊，不需要绚丽的UI，只要实用就行。
谈到实用，最典型的例子就是排序一个文件，Linux下只需要一个sort命令就够了。
使用图形界面工具可能比较麻烦，而且占用资源也多。
这篇记录了一些常用的命令行技巧，多是来自技术问答网站，比如stackoverflow。

每个小贴士都基本分为三个部分，即问题、简介和实例。
每个标题都是一个问题，我尽量让这些问题能概括后面的内容，并做到简洁明了。
问题下面会有简单背景介绍，例如需要解决的问题和使用中需要注意的问题。
最后是实际例子是最重要的部分，对于每个例子都会有相应的解释。
除了上面三个部分，可能还有额外的部分，例如参考文献以及可能的讨论等。


<!-- more -->

## 在二、八、十、十六进制之间快速反复横跳？
最近一段时间在研究各类压缩文件格式，涉及很多字节上的操作。
常常需要在不同进制之间反复横跳，网页搜索太麻烦、自己写工具需要考虑太多。
索性问谷歌怎么能实现命令行下快速反复横跳，谷歌也是爽快，直接给了答案。

```{bash}
# bc是一个命令行计算器（的确是计算器）。但我们可以借助它不同进制之间反复横跳。
# 默认情况下，输入是十进制。所以，转换十进制到其它进制时可简写为：
$> bc <<<"obase=2;10"
$> bc <<<"obase=8;10"
$> bc <<<"obase=10;10"
$> bc <<<"obase=16;10"

# 通过ibase可设置输入的进制。
$> bc <<<"ibase=16;obase=2;A"
$> bc <<<"ibase=16;obase=8;A"
$> bc <<<"ibase=16;obase=10;A"
$> bc <<<"ibase=16;obase=16;A"

# 更骚的是你可以随意设置base。例如，我们假设输入是16进制，然后创造一个3进制的系
# 统，可用以下命令行实现。
$> bc <<<"ibase=16;obase=3;A"
```


[^1]: [bc command in Linux with examples](https://www.geeksforgeeks.org/bc-command-linux-examples)

## 用命令行录屏
俗话说的好，一图胜千言，一V胜千图。在演示某些操作时，使用GIF图片动态地展示该过程，方便理解和交流。
在Linux下可以用`ffmpeg`命令实现，且可以通过不同的参数实现个性化的录制。

`ffmpeg`的安装很简单，如果有管理员权限，`Debian`系发行版本可直接用`apt`安装。
```
sudo apt-get install ffmpeg
```

如果没有，可以编译安装，详见

```{bash}
ffmpeg -video_size 1920x1080 -framerate 10 -f x11grab -i :0.0+100,200 screen.gif
```

## `curl`

`curl`是一个很强大的下载工具，此处记录若干使用的技巧。


### 批量下载

```bash
# https://stackoverflow.com/a/51684116/11203559
curl --fail \
  --max-filesize 500 \
  --write-out "%{http_code}\t%{url_effective}\n" \
  -o '#1.dmg' \
  'http://fmdl.filemaker.com/maint/107-85rel/fmpa_17.0.2.[200-210].dmg'
```

## `ssh`

### 端口转发

在使用`ssh`命令时，我们使用`-L`参数实现**本地端口转发**，即将发送到本地端口的请
求，转发到目标端口。基本的数用语法如下：

```
ssh -L 本地地址:本地端口:目标地址:目标端口 目标主机地址
```

示例：假设远程主机地址为server.hpc.com，登陆用户名为user，本地端口和远程端口都
为10086（即发送到本地端口10086的内容会被转发到远程主机的10086端口）

```
ssh -L 10086:server.hpc.com:10086 user@server.hpc.com

# 如果是本地转发，本地地址可以省略
ssh -L localhost:10086:server.hpc.com:10086 user@server.hpc.com
```

有时需要**远程端口转发**，即将发送到远程A端口的请求，转发到目标B端口，用`-R`参
数实现。基本语法如下：

```
ssh -R 远程地址:A端口:远程地址:B端口 远程地址
```

示例：假设远程主机地址为server.hpc.com，登陆用户名为user，端口A为10086，端口B为
10010（即发送到远程主机端口10086的内容会被转发到该主机的10010端口）

```
ssh -R localhost:10086:localhost:10010 user@server.hpc.com
```

此外还有动态端口转发和链式端口转发，后续会补充(**TODO**)。

参考：  
1. [玩转SSH端口转发](https://blog.fundebug.com/2017/04/24/ssh-port-forwarding)

<!-- vim: set nospell: -->
