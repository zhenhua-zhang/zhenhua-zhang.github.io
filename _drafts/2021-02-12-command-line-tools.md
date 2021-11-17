---
title: 我Linux下命令行使用笔记
date: 2021-02-12
tags: [linux,cli]
categories: [cli]
---

## 文本三剑客grep、sed和awk
`grep`是Unix操作系统下的一个用于文本搜索的命令行工具，由贝尔实验室于1974年开发，后移植到类Unix系统，如Linux等。
与`sed`和`awk`并称文本三剑客，

## Reference
[^1]: [Linux文本三剑客超详细教程](https://www.cnblogs.com/along21/p/10366886.html)

## Linux下如何安装Tex Live

LaTex作为一个怪兽级别的排版系统，有针对不同平台的发行版本，Linux平台下则是著名的(TexLive)[https://www.tug.org/texlive/quickinstall.html]。
Linux下安装软件既可以通过管理员权限从源下载安装，也可以通过编译安装。
而对于TexLive，用户可以在不需要管理员权限的情况下通过Perl脚本安装。
安装过程是交互的，用户可以指定某些路径和配置等。
下面我们介绍安装过程和基本的配置。

### 清理旧的安装
```
# 可能需要管理员权限
rm -fr /usr/local/texlive/2020
rm -fr ~/.texlive2020
```

### 下载安装脚本并安装
我们可以从官方提供的ftp站点下载前面提到的安装脚本[install-tl](https://ftp.snt.utwente.nl/pub/software/tex/systems/texlive/tlnet/)。
这是一个Perl脚本，因此需要系统中一安装Perl（一般而言，每个Linux发行版本默认带Perl解释器）。
安装过程是交互的，并且有菜单提示我们可以设置的内容，如下：

![install-tl-scripts-main-menu.png](images/)

另外，安装的界面由三种选择，包括纯文本界面，图形界面和批处理界面。
最后，由于网络限制，默认下载源可能速度很慢，我们可以用下面的命令来修改该下载源
```
perl ~/tools/bin/install-tl --location http://mirror.example.org/ctan/path/systems/texlive/tlnet
```

### 设置`PATH`环境变量
在有管理员权限的情况下，上面的包会被安装到默认路径，一般为`/usr/local/texlive`。
当不使用管理员权限的时候，需要用户指定自己的安装路径，这样就需要用户在完成安装后自行指定Tex Live相关的可执行程序。
需要把如下语句加到自己使用的shell的配置文件中。
```
export PATH=$PATH:/path/to/where/the/texlive/installed
```

### 使用`tlmgr`管理安装的包
安装完毕并配置环境变量后，我们可以通过`tlmgr`（the native TeX Live Manager）来管理和更新已安装的包，并可以方便的安装新的包，以下为几个常用的例子。

1. 设置包源
```
tlmgr options repository ctan
tlmgr options repository http://mirror.ctan.org/systems/texlive/tlnet
```

2. 升级包
```
# 查看可升级的包
tlmgr update --list

# 升级所有的包
tlmgr update --all

# 查看某个包的细节
tlmgr info PACKAGE-NAME
```

3. 安装、删除包
```
tlmgr remove PACKAGE-NAME
tlmgr install PACKAGE-NAME
```
