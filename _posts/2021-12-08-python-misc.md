---
title: Python杂谈
date: 2021-11-08 19:32:21
tags: [python,python3,pip]
categories: [Python]
---

# Python 杂谈

主要记录与Python相关的常见病症及疑难杂症，不确保病症的普遍性。

## Debian下`venv`安装虚拟环境并使用`pip`安装其它包

Python 3标准库中的`venv`包是创建虚拟环境的轻量工具，而`pip`是PyPA推荐的安装Python包的工具。
两者相互配合可以创建非常方便且互相独立的Python开发环境。
但在Debian中Python与其大部分包是分开的，好处是简化安装和保证系统安全性，坏处是自行安装可能费时费力。
最近在测试自己写的一个工具，需要用到虚拟环境并安装一些包，但`pip`似乎有些问题。
主要表现为安装Debian源里的`python3-pip`包后，命令行下使用`pip`会报如下的错误：

<!-- more -->

```
$> which pip
/usr/bin/pip  # 不同版本路径可能不同
$> pip
Traceback (most recent call last):
  File "/usr/bin/pip3", line 33, in <module>
      sys.exit(load_entry_point('pip==20.3.4', 'console_scripts', 'pip3')())
  File "/usr/bin/pip3", line 25, in importlib_load_entry_point
    return next(matches).load()
  File "/usr/lib/python3.9/importlib/metadata.py", line 77, in load
    module = import_module(match.group('module'))
  File "/usr/lib/python3.9/importlib/__init__.py", line 127, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 1030, in _gcd_import
  File "<frozen importlib._bootstrap>", line 1007, in _find_and_load
  File "<frozen importlib._bootstrap>", line 986, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 680, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 790, in exec_module
  File "<frozen importlib._bootstrap>", line 228, in _call_with_frames_removed
  File "/usr/lib/python3/dist-packages/pip/_internal/cli/main.py", line 10, in <module>
    from pip._internal.cli.autocompletion import autocomplete
  File "/usr/lib/python3/dist-packages/pip/_internal/cli/autocompletion.py", line 9, in <module>
    from pip._internal.cli.main_parser import create_main_parser
  File "/usr/lib/python3/dist-packages/pip/_internal/cli/main_parser.py", line 7, in <module>
    from pip._internal.cli import cmdoptions
  File "/usr/lib/python3/dist-packages/pip/_internal/cli/cmdoptions.py", line 23, in <module>
    from pip._vendor.packaging.utils import canonicalize_name
ModuleNotFoundError: No module named 'pip._vendor.packaging'
```

可能是我从Debian 10升级到11过程中出了岔子，期间一不小心删除了`/usr/bin/`下的所有东西，幸好折腾了一晚上，算是救回来了。

上面的报错，我用谷歌和百度都没找到合适的解决方法，搜索起来比较困难的原因应该是Python下打包命令行工具时策略的问题。
简单来说是，使用相同的方式打包，在报错时Traceback太冗长，不容易分辨哪个是主要信息，从而导致搜索时失去重点。

于是我开始曲线救国。

首先卸载全局pip，即Debian的`python3-pip`包：

```
$> sudo apt purge python3-pip # 使用purge，避免配置文件残余带来的干扰。
```

然后用`venv`构建一个虚拟环境：

```
$> python3 -m venv .env # 一般安装python时会同时安装其标准库，其中也包含venv
$> source .env/bin/activate
```

使用`Python`手册中推荐的方式安装`pip`

第一种是用`ensurepip`，这是`Python`官方手册的方法

```
(.env) $> python -m ensurepip

# 或者
(.env) $> python -m ensurepip --upgrade
```

但上面的方法在Debian中是被禁止的，所以我们需要用第二种方法。

```
(.env) $> curl -sS https://bootstrap.pypa.io/get-pip.py | python3
(.env) $> python -m pip install torch
```


简单总结起来就是，至少两种情况下上面的方法是有意义的

1. 自己没有管理员权限且系统的`Python`没安装`pip`
2. 像我遇到的情况，有管理员权限且能安装pip，但使用时报错

然后先创建虚拟环境，在虚拟环境中用`pip`的官方指导方法安装`pip`，该`pip`是属于当前虚拟环境的，只用在激活该虚拟环境的情况下才会有用，因而不会影响其它的`pip`
