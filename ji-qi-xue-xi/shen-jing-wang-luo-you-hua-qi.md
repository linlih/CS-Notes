# 神经网络优化器

| 类别 | 优化器 |  |
| :--- | :--- | :--- |
| 固定学习率 | BGD（批梯度下降） |  |
|  | SGD（随机梯度下降） |  |
|  | MBGD（小批量梯度相加） |  |
|  | Momentum |  |
|  | Nesterov Momentum |  |
| 动态学习率 | Adagrad |  |
|  | RMSprop |  |
|  | Adam |  |

## BGD

更新规则如下：

$$
\theta_j:=\theta_j-\alpha\frac{\partial J(\theta)}{\partial\theta_j}\\
\frac{\partial J(\theta)}{\partial\theta_j}=\frac{1}{m}\sum_{i=1}^{m}(h_{\theta}(x_i)-y_i)^2
$$

其中m表示的是样本数，所以在BGD中，没更新一个参数，需要遍历所有的样本，有n个参数，则更新一次参数需要遍历n次样本，使得训练速度非常慢，所以BGD不适合大数据集的训练。

## SGD

$$
\theta_j:=\theta_j-\alpha(h_{\theta}(x_i)-y_i)
$$

只根据一个样本就更新参数，速度就非常快了，但是这样也会引入新的问题，就是每次更新的方向不一定是朝着最优的方向，所以训练的准确度下降，不一定能够得到全局最优。

## MBGD

MBGD则是综合了BGD和SGD的特点，每次选择所有样本的一个小批量来更新参数。

优点：收敛更稳定，另一方面可以充分地利用深度学习库中高度优化的矩阵操作来进行更有效的梯度计算。 缺点：关于学习率的选择，如果太小，收敛速度就会变慢，如果太大，loss function就会在极小值处不停的震荡甚至偏离。

## Momentum

这个思想是结合了物体运动的惯性问题，当一个球体从山坡上滚下的时候，当到达的一个低谷的时候，由于惯性的存在会继续保持向前的动力，在低谷的时候会来回滚动，最终停止。这样有什么好处呢？在神经的网络的训练中，损失平面是凹凸不平的，那么会有很多的低谷，我们的目标是找到海拔最低的那个低谷，但是很多时候可能陷入一个低谷，无法跳出来训练的损失无法继续下降，但是这个低谷并不是最优的，引入了动量，在进入这个低谷的时候，由于保持着一定的惯性，就有概率能够跳出这个低谷，从而继续寻找最优的低谷。同样的，这样也会引入新的问题，就是**overshoot**，就是假设我们已经找到了最优的低谷，但是由于惯性的存在，我们却跳出了这个低谷，导致整体的性能下降。



## Adagrad

[https://www.cnblogs.com/yunqishequ/p/10026758.html](https://www.cnblogs.com/yunqishequ/p/10026758.html)

## RMSprop



## Adam



ref:

[https://blog.csdn.net/weixin\_29260031/article/details/82320525](https://blog.csdn.net/weixin_29260031/article/details/82320525)

