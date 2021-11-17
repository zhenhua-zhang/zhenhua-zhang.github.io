---
title: Transformer
date: 2021-11-16 20:40:00
tags: [deep-learning,nlp]
categories: [DeepLearning]
mathjax: true
---

# Transfromer

## 序列转译（Seq2seq）模型

序列转译（sequence-to-sequence）常用于机器翻译，图像语义分割和文本总结。序列转译模型把一个序列转换成另一个序列。模型由两个基本模块组成：编码器（encoder）和解码器（decoder）。

以机器翻译为例，输入序列是源语言，经过序列转译模型处理，输出目标语言，如英译中。
基本的转译过程如下：首先，编码器把输入序列编译为上下文（context），然后由解码器把该上下文解译并产生输出序列。
实际上，此处的上下文是一个向量，大小是随意的，比如256。而编码器和解码器都是循环神经网络（recurrent neural network）。

<!-- more -->

## 循环神经网络

序列信息存在上下文关系，比如自然语言。（高中现代文阅读中的联系上下文理解词语的意思🌚）
而普通的神经网络（比如全链接前馈神经网络）没法捕捉到序列中相邻元素之间的关系，即模型在更新权重矩阵时，仅考虑当前（$T_i$）输入的数据，而忽略了在此之前数据。虽然前面的数据（$T_0-T_{i-1}$）在其输入时也更新了权重矩阵。

循环神经网络能把序列各元素之间的

$$\begin{equation} \label{eq2}
\begin{aligned}
a &= b + c \\
  &= d + e + f + g \\
  &= h + i
\end{aligned}
\end{equation}$$

## Attention mechanism

## Transformer

目前也没有统一的中文翻译，而且翻译后可能更难理解，因此直接使用transformer更好。

## 参考文献

1. Transformer (machine learning model): https://en.wikipedia.org/wiki/Transformer_(machine_learning_model)
2. Visualizing a neural machine translation model: https://jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention
