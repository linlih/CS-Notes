# 拉格朗日乘子法和KKT条件

拉格朗日乘子法用于有条件的优化，具体的优化的场景如下：

$$
\begin{align} 
min&\ f(x) \\
s.t.&\  h(x) = 0 \\
      &\ g(x) \leq 0
\end{align}
$$

## 等式约束条件

这里我们需要分情况讨论，首先我们来看条件为$$h(x)=0$$的情况，例子如下：

$$
\begin{align} 
min&\ f(x)=x_1 + x_2\\
s.t.&\  h(x) = x_1^2 + x_2^2 - 2 = 0 \\
\end{align}
$$

首先我们可以绘制出$$f(x)=x_1+x_2$$的等高线，**等高线的概念是指在这条线上，**$$f(x)$$**的函数值是一样的**，如图中的 线所示，接下来对$$f(x)$$进行求导$$\nabla f(x)$$，得到下面的结果：

$$
\frac{\partial f(x_1, x_2)}{\partial x_1} = 1\\
\ \\
\frac{\partial f(x_1, x_2)}{\partial x_2} = 1 \\
$$

所以最终我们得到$$f(x)$$的导数为：

$$
\nabla f(x) = \begin{pmatrix} 1 \\ 1  \end{pmatrix}
$$

对应的是图上  的向量。

下面我们看下$$\nabla h(x)$$

$$
\frac{\partial h(x_1, x_2)}{\partial x_1} = 2x_1\\
\ \\
\frac{\partial h(x_1, x_2)}{\partial x_2} = 2x_2 \\
$$

最终得到的$$h(x)$$的导数为

$$
\nabla h(x) = \begin{pmatrix} 2x_1 \\ 2x_2  \end{pmatrix}
$$







## 不等式约束条件

现在我们来看不等式的约束方式

$$
\begin{align} 
min&\ f(x) = x_1 + x_2\\
s.t.&\  g(x) = x_1^2 + x_2^2 - 2 \leq 0 \\
\end{align}
$$

不等式的情况可以分为两种：











