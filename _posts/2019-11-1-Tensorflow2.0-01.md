---
layout: post
title:  自学TensorFlow2.0
date:   2019-11-1 14:23:5 +0800
categories: TensorFlow2.0 深度学习
tags: TensorFlow2.0 深度学习
author: gzf
excerpt: 学习TensorFlow2.0，经验积累
mathjax: true
---

## 自学 TensorFlow2.0 (1)



### 问题解决
> 记录学习过程中遇到的问题

#### 1、tensor 切片
```python
tf.slice(
    input_,
    begin,
    size,
    name=None
)
```
> 一个张量`input_`，从`begin`指定的位置开始提取一个大小为`size`的切片。
> 注意：`tf.Tensor.getitem`是一个更符合**python**风格的切片方式，推荐使用`foo[3:7, :-2]`而不是`tf.slice(foo, [3, 0], [4, foo.get_shape()[1]-2])`.
> 尤其是当tensor中有的维度是None时，此时`tf.slice`不能工作。

#### 2、tf.reshape()
> `tf.reshape` takes a tensor of ints as the shape argument. If you are passing None, it doesn't work. Maybe you want to replace `x.get_shape()[0]` with `tf.shape(x)[0]`
> 当应用`tf.reshape()`的张量中有的维度是None时，请使用`tf.shape(x)[0]`
