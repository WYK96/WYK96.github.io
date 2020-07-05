---
title: 几种常见的激活函数
tags: [深度学习相关]
style: fill
color: info
description: ReLU/ReLU6/LeakyReLU/PReLU/Sigmoid/tanh.
---

激活函数在神经网络中起到了重要的作用，如果没有激活函数，层数再多的网络也只是线性模型，很可能无法正确地对复杂模式进行学习。

激活函数作用于每一层节点的输出之后，通过引入激活函数，网络具有了非线性关系的能力，从而获得更好的特征表达。接下来，介绍几种常见的激活函数：

# ReLU


$$
y=
\begin{cases}
x, x > 0 \\
0, x \le 0
\end{cases}
$$

<img src="/blog_resources/active_function/relu.png" width="95%">


# ReLU6

$$
y=
\begin{cases}
6, x > 6  \\
x, 0 < x \le 6 \\
0, x \le 0  \\
\end{cases}
$$

<img src="/blog_resources/active_function/relu6.png" width="95%">

# LeakyReLU

$$
y=
\begin{cases}
x, x > 0  \\
ax, x \le 0\ and\ a\ is\ fixed
\end{cases}
$$

<img src="/blog_resources/active_function/leakyrelu.png" width="95%">

# PReLU

$$
y=
\begin{cases}
x, x > 0  \\
ax, x \le 0\ and\ a\ is\ learnable  
\end{cases}
$$
<img src="/blog_resources/active_function/prelu.png" width="95%">

# Sigmoid

$$
y= \frac{1}{1+e^{-x}}
$$

<img src="/blog_resources/active_function/sigmoid.png" width="95%">

# tanh

$$
y= \frac{1 - e^{-2x}}{1 + e^{-2x}}
$$

<img src="/blog_resources/active_function/tanh.png" width="95%">