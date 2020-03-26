# 数学基础

## Jensen's inequality

![](../.gitbook/assets/image%20%2810%29.png)

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
f\Big(\int \boldsymbol xp( \boldsymbol x) d \boldsymbol x\Big) \leq \int f( \boldsymbol x)p( \boldsymbol x)d \boldsymbol x
$$

## Jocobian矩阵&Hessian矩阵

Jocobian矩阵是由一阶偏导数构成的

Hessian矩阵是由二阶偏导数构成的

参考：

[Jacobian矩阵和Hessian矩阵](https://www.cnblogs.com/wangyarui/p/6407604.html)



## 梯度

在机器学习中，我们想要优化对应的损失函数，在损失平面上希望每次移动的方向是朝着下降最快的方向移动，这样才能够最快找到最优解。这一个方向称之为梯度。

梯度的计算就是求函数各个分量的偏导数。

参考：

[为什么梯度反方向是函数值局部下降最快的方向？](https://zhuanlan.zhihu.com/p/24913912)





