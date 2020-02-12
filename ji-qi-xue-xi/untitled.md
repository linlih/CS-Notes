# 拉格朗日乘子法和KKT条件

拉格朗日乘子法用于有条件的优化，具体的优化的场景如下：

$$
\begin{align} 
min&\ f(x) \\
s.t.&\  h(x) = 0 \\
      &\ g(x) \leq 0
\end{align}
$$

这里我们需要分情况讨论，首先我们来看条件为$$h(x)=0$$的情况，例子如下：

$$
\begin{align} 
min&\ f(x)=x_1 + x_2\\
s.t.&\  h(x) = x_1^2 + x_2^2 - 1 = 0 \\
\end{align}
$$







