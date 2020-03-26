# 神经网络

## 反向传播算法

核心的思想在于理解从x-&gt;y正向是计算损失，然后倒过来从y-&gt;x计算误差，利用链式法则计算参数的倒数，从而完成更新，迭代这个计算过程。

其中在链式法则计算的过程中，比较难理解是各个变量的依赖关系。

 对于上层节点p和下层节点q，要求得![\frac{\partial p}{\partial q}](http://zhihu.com/equation?tex=%5Cfrac%7B%5Cpartial+p%7D%7B%5Cpartial+q%7D)，需要找到从q节点到p节点的所有路径，并且对每条路径，求得该路径上的所有偏导数之乘积，然后将所有路径的 “乘积” 累加起来才能得到![\frac{\partial p}{\partial q}](http://zhihu.com/equation?tex=%5Cfrac%7B%5Cpartial+p%7D%7B%5Cpartial+q%7D)的值。

参考：

[“反向传播算法”过程及公式推导](https://blog.csdn.net/ft_sunshine/article/details/90221691)

[反向传播算法（过程及公式推导）](https://www.cnblogs.com/wlzy/p/7751297.html)

[一文弄懂神经网络中的反向传播法——BackPropagation](https://www.cnblogs.com/charlotte77/p/5629865.html)  （包含示例代码）

