---
title: Python中格式化字符串的13个例子
date: 2020-08-18 19:32:21
tags: [python,python3,string-format]
categories: [Python]
---

Python中字符串格式化的用法。

## 基本用法

```{python}
string = '%s %s' % ('one', 'two')
string = '{} {}'.format('one', 'two')

integer = '%d %d' % (1, 2)
integer = '{} {}'.format(1, 2)

# 新的格式化风格还可以指定参数的位置
string = '{1} {0}'.format('one', 'two')
```

<!--more-->

## 值的转换

新的格式化风格默认调用实例的`__format__()`方法。使用以下方法输出`str()`和`repr()`的作用结果。

``` {python}
class Data(object):
    def __str__(self):
        return 'str'

    def __repr__(self):
        return 'repr'

print('%s %r' % (Data(), Data()))
print('{0!s} {0!r}'.format(Data()))

# 此外Python3中还有另一种转换方式，如下
print('%r %a' % (Data(), Data()))
print('{0!r} {0!a}'.format(Data()))
```

## 填充对齐

```
# 默认情况下格式化的字符串与原字符串的长度相同，但用户可根据实际需求添加填充符以
# 使字符串达到所需的长度。旧的格式化方法与新的格式化方法在语法上查表较大。如下：

# 右侧对齐
print('%10s' % ('test',))
print('{:>10}'.format('test'))

# 右侧对齐
print('%-10s' % ('test',))
print('{:<10}'.format('test'))
print('{:10}'.format('test'))

# 以下三种方法不适用旧的格式化方法
# 使用其它字符填充
print('{:_<10}'.format('test'))

# 字符串居中
print('{:^10}'.format('test'))

# 当需要填充的字符数目无法被2整除，较多的填充字符放置在字符串右侧
print('{:^6}'.format('zip'))
```

## 字符串截断

```
# 当字符串较长且无需全部输出时，输出该字符串的一部分是较好的选择。
print('%.5s' % ('xylophone',))
print('{:.5}'.format('xylophone',))
```

## 填充和截断

```
# 可以同时使用填充和截断。一般时先截取字符串，然后用指定字符做填充
print('%-10.5s' % ('xylophone',))
print('{:10.5}'.format('xylophone'))
```

## 数字格式化

```
# 格式化数字的例子如下
# 整数
print('%d' % (42,))
print('{:d}'.format(42))

# 转为二进制
print('%b' % (42,))
print('{:b}'.format(42))

# 转为八进制
print('%o' % (42,))
print('{:o}'.format(42))

# 转为十六进制
print('%x' % (42,))
print('{:x}'.format(42))

# 浮点数
print('%f' % (3.141592654, ))
print('{:f}'.format(3.141592654, ))
```


## 数字对齐填充

``` 
# 数字的填充和字符串的填充类似
print('%4d' % (42,))
print('{:4d}'.format(42))

# 与字符串截取类似，可取小数点后指定位数
print('%06.2f' % (3.141592654, ))
print('{:06.2f}'.format(3.141592654))

# 在新的格式化语法中，禁止取整型变量小数点后位数
```

## 数字的符号

```
# 默认只有负数在输出时带有符号，但格式化的语法中可更改
print('%+d' % (42,))
print('{:+d}'.format(42))

# 用空格表明输出负数时在该数字前添加符号。但被格式化数字为正数时，空格原义输出。
print('% d' % (42,))
print('{:d }'.format((-23)))

# 新的字符串格式化的方法中还可以设置符号和数字间的填充
print('{:=5d}'.format((-32)))  # 整个字符串输出占位5个，符号和数字间有两个空格填充
print('{:=+5d}'.format(23))    # 当数字为正数时，需要著名符号，输出才显示。
```


## 变量名占位

```
# 新旧两种方法都支持用字典指定输出值
data = {'first': 'Hodor', 'last': 'Hodor!'}

print('%(first)s %(last)s' % data)
print('{first} {last}'.format(**data))

# 同时新的方法还支持参数指定
print('{first} {last}'.format(first='Hodor', last='Hodor!'))
```

## `Getitem`和`Getattr`

```
# 新的格式化方法还支持任何实现了__getitem__()方法的类或数据类型
# dict
person = {'first': 'Jean-Luc', 'last': 'Picard'}
print('{p[first]} {p[last]}'.format(p=person))

# list
data = [4, 8, 16, 32, 64, 128]
pirnt('{d[4]} {d[5]}'.format(d=data))

# 自定义实现__getattr__()的类
class Plant(object):
    type = 'tree'

print('{p.type}'.format(p=Plant()))

# 前面两种方法可混合使用
class Plant(object):
    type = 'tree'
    kinds = [{'name': 'oak'}, {'name': 'maple'}]

print('{p.type}: {p.kinds[0][name]}'.format(p=Plant()))
```

## 日期格式化

```
# 新的格式化方法还支持对象自带呈现方式，如时间的格式化输出

from datatime import datetime
print('{:%Y-%m-%d %H:%M}'.format(datatime(2020, 8, 18, 21, 49)))
```

## 参数控制的格式化

```
# 新的格式化方法还支持动态设置格式化参数，如对齐方式、填充字符
print('{:{align}{width}}'.format('test', align='^', width='10'))

print('%.*s = %.*f' % (3, 'Gibberish', 3, 2.7182))

# 指定一次参数，可重复使用
print('{:.{prec}} = {:.{prec}f}'.format('Giberish', 2.7182, prec=3))

# 同时设置宽度和精度
print('%*.*f' % (5, 2, 2.7182))
print('{:{width}.{prec}f}'.format(2.7182, width=5, prec=3))

# 另一种方法（仅适用于新的格式化方法）
print('{:{prec}} = {:{prec}}'.format('Gibberish', 2.7182, prec='.3'))

# 时间的格式化可分开设置
from datetime import datetime
print('{:{dfmt} {tfmt}}'.format(dt, dfmt='%Y-%m-%d', tfmt='%H:%M'))

# 此外还可以使用位置参数设置。需要注意输出数据和格式化控制符的顺序，比较繁琐，
# 不建议使用。
print('{:{}{}{}.{}}'.format(2.7182818284, '>', '+', 10, 3))

# 混合用法。此处需要遵循先位置参数后关键词参数的顺序。
print('{:{}{sign}{}.{}}'.format(2.7182818284, '>', 10, 3, sign='+'))
```

## 自定义对象

```
# datetime能够被格式化输出的原因是因为它实现了__format__()“魔术”方法。也就是说
# 凡是实现了该方法的类都可以使用前面所述的例子格式化输出。

class HAL9000(object):
    def __format__(self, format):
        if format == 'open-the-pod-bay-doors':
            return "I'm afraid I can't do that."
        return 'HAL 9000'

print('{:open-the-pod-bay-doors}'.format(HAL9000()))
```

<!-- vim: set ai nospell ts=4 tw=500 ft=pandoc: -->
