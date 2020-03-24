# 数学基础

## Jensen's inequality

![](../.gitbook/assets/image%20%287%29.png)

首先要明确下凸函数的概念：每条弦都位于图像或其上方，就称这个函数是凸函数。

从数轴的$$x=a$$到$$x=b$$中间的所有值可以写成$$\lambda a+(1-\lambda)b$$，其中$$0\leq \lambda \leq 1$$。弦\(图中的蓝色直线\)上的点可以写成$$\lambda f(a) +(1-\lambda)f(b)$$，凸函数对应的值为$$f(\lambda a +(1-\lambda)b)$$，这样，凸函数的性质可以写成：

$$
f(\lambda a +(1-\lambda)b)\leq\lambda f(a) +(1-\lambda)f(b)
$$

根据数学归纳法，可以得到：

$$
f(\sum _{i=1}^m\lambda_ix_i)\leq\sum _{i=1}^m\lambda_if(x_i)
$$

如果把$$\lambda_i$$看成是取值为$$\{x_i\}$$的离散变量$$x$$的概率分布的话，那么上述的公式可以写成：

$$
f(\mathbb{E}[x]) \leq \mathbb{E}[f(x)]
$$

其中$$\mathbb E[\cdot]$$表示的是期望。对于连续变量，可以写成：

$$
f(\int \boldsymbol xp( \boldsymbol x))d \boldsymbol x \leq \int f( \boldsymbol x)p( \boldsymbol x)d \boldsymbol x
$$





