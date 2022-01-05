---
title: Linux bash脚本编程
date: 2020-08-18 19:32:21
tags: [linux,bash]
categories: [Linux]
---

## 文本三剑客

## 内置命令和系统令名

## 声明变量

## 流程控制

## 关系数组

```
# 声明一个关系数组并命名为my_array
declare -A my_array

# 添加一对键值
my_array["key1"]="val1"

# 获取某个键所指向的值
echo ${my_array["key1"]}

# 查看所有键和值
echo ${my_array[@]}
echo ${!my_array[@]}

# 用for循环遍历键或值
for key in ${!my_array[@]}; do
  echo key
done

for val in ${my_array[@]}; do
  echo val
done
```
