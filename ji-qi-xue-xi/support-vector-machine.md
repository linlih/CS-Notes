# SVM

## SVM原理

首先需要知道支持向量机的分类：

1. 线性可分支持向量机

   这种向量机是属于最为理想的状态，满足两个条件，线性和可分。

2. 线性支持向量机

   这种向量机则允许出现一些错误的数据点

3. 非线性支持向量机

   这种向量机就是能够处理非线性的数据，通过特征映射的方式将非线性的数据信息转化为线性可分的数据，然后再利用向量机进行分类。

简单的理解表现在二维平面上就是线性的分类方式为一条直线，而非线性的分类方式为一条曲线。

## 函数间隔与几何间隔：

### 参数说明

线性核：

C参数越来越大，分类的界面越来越窄，表明使用了越来越少的样本来做分类，有可能导致过拟合，但是C值越大，能使得训练精度变高。实际中需要确认想要的目标是训练精度高，还是泛化能力强的问题

高斯核：

γ足够大的话，分界曲面靠近于训练数据，换句话说，总是可以将γ值取得足够大的话，使得它的分类准确度是百分之一百

当γ足够小的时候，高斯核就退化成为了线性核 当γ足够大的时候，高斯核就退化成为了K近邻

## SVM优化问题推导

假设在一个平面中，存在两个类别的样本，并且是可分的，那么一定可以找到无数条直线将其分开。存在一条直线可以将其分开，并且这条直线离样本的距离是最大的。

下面我们通过一个实际的表达式来推到到数学上的表达式

假设这条直线为：$$2x + y  -4 = 0$$

那么我们可以写成$$(2,1)\begin{pmatrix}x \\ y\end{pmatrix} - 4 = 0$$

即将$$(2, 1)$$作为$$w$$，$$\begin{pmatrix}x \\ y\end{pmatrix} $$作为$$x$$，$$-4$$作为$$b$$，这样分界面就可以写成$$w^T x + b = 0$$。其中$$w$$和$$x$$为向量，$$b$$为标量。这里的分界面的表达式代表的就是这条分解的直线，在高维空间中，则称之为**超平面**。

此时将样本点带入$$w^Tx + b$$中得到的结果如果为正数，样本点就位于直线法线的正向，反之则位于法线的负向。

接下来我们要考虑的是如何衡量样本属于正例还是负例呢？

这里使用的是距离的方式来衡量，回顾下我们计算点到直线的距离计算公式：$$\frac{|wx + b|}{\left \| w \right \|}$$，通过这个来衡量样本分界面$$w^Tx + b = 0$$的距离。

但是这个公式中包含了一个绝对值让人很讨厌，我们要想办法去掉绝对值。

如果该样本属于正例的话，那么$$w^Tx + b$$的结果为一个正数，如果是负例的话，则是一个负数，那么这个时候如果我们给$$w^Tx + b$$乘上标记值$$y$$，当样本点落在直线上方的时候，距离为正值，对应的标记值$$y$$为+1，**二者相乘为正值**。如果样本点位于直线下方，距离为负值，对应的标记值$$y$$为-1，**二者相乘也为正值**。那就可以去掉$$w^Tx + b$$的绝对值符号了，结果为$$ \frac { y*(w^Tx + b)} {\left \| w \right \|} $$, 这就是最终的距离衡量式子。

那么我们要的目标是什么呢？

我们希望的是能够找到一个分割面，二维的话就是一条直线，能够使得样本离这条直线的距离最大。

那么在训练过程中，我们首先需要找到的是离分界面距离最小的那个样本，然后使得这个样本到超平面的距离达到最大，表达成数学式子如下：

$$
{\underset {w, b}{arg\, \operatorname {max} }}\,\{  \frac { 1} {\left \| w \right \|}{\underset {n} {\operatorname {min} }}\,  y_n*(w^Tx_n + b)\}
$$

因为$$\frac{1}{\left \| w \right \|}$$和n无关，所以提出到外面。

上面的目标函数中：有这一个$$\frac{1}{\left \| w \right \|}$$项，这个$$\left \| w \right \|$$的计算是这样的，假设它为k维，那么$$\left \| w \right \| = \sqrt {w_1^2+w_2^2+w_3^2+...+w_k^2}$$再开根号，如果直接用这个来进行优化，实现起来非常复杂，所以需要做一个等价的变化。

比如说刚刚的直线：$$2x + y - 4 = 0$$，那么我们总是可以计算所有的样本到这条直线的距离，那么必然存在最小距离，我们可以将这个距离进行缩放到1，这个是可以做到的。

原因是这样，假设直线的最小距离算出来为7，那么我们对直线距离计算公式两边同时除以7，那么就可以让距离变为1，那么此时的直线可能转化为 $$\frac{2x + y -4}{4}= 0$$。那么这个时候$$ y*(wx + b)$$这个项就为1，同时要求所有的样本点的y\*\(wx + b\)计算的结果都要大于等于1。

那么我们就可以令离超平面最近的点的距离为：$$ y*(wx + b)=1$$，这样所有的样本点到超平面的距离都会大于等于1，所以$$ {\underset {n} {\operatorname {min} }}\,  y_n*(wx_n + b) = 1$$。

那么目标函数就可以化简为：$$arg\, {\underset {w}{\operatorname {max} }}\,   \frac{1}{\left \|w \right ||}\\ s.t. \ \  y_n*(w^Tx_n + b) \geq 1$$

此时我们多了一个约束条件，$$ y*(wx + b)$$大于等于1。

要求$$\frac{1}{\left \| w \right \|}$$最大，那就是要求$$ \left \| w \right \|$$最小，就相当于求$$ \frac{1}{2} \left \| w \right \|^2$$最小，其中的$$\frac{1}{2}$$和平方都是为了后面的求解方便。

最终我们得到SVM的目标函数为：

$$
arg\, {\underset {w}{\operatorname {min} }}\,    \frac{1}{2} \left \| w \right \|^2\\ s.t. \ \  y_n*(w^Tx_n + b) \geq 1, \ \ n=1, ... ...,N
$$

## 拉格朗日乘子法和KKT条件

那么现在怎么做呢？带条件的最优化问题，需要用到拉格朗日乘子法。关于拉格朗日乘子法为什么要这么做，参考[这里](lagrange-kkt.md)。

引入拉格朗日乘子，将优化函数写成：

$$
L(w,b, \alpha)=\frac{1}{2} \left \| w \right \|^2 +\sum_{n=1}^N\alpha _n\{1-y_n(w^Tx_n+b)\}
$$

然后对$$w$$和$$b$$分别求偏导得到：

$$
\frac{\partial L(w, b, \alpha)}{\partial w}=w-\sum_{n=1}^N\alpha _ny_nx_n=0\\
\frac{\partial L(w, b, \alpha)}{\partial b}=\sum_{n=1}^N\alpha _ny_n=0
$$

化简一下可以得到：

$$
w=\sum_{n=1}^N\alpha _ny_nx_n\\
\sum_{n=1}^N\alpha _ny_n=0
$$

将这两个结果带入先前的拉格朗日函数中：

$$
\begin{aligned}
L(w,b,\alpha )&=\frac{1}{2}\Vert w\Vert ^{2} +\sum ^{N}_{n=1} \alpha _{n} \{1-y_{n} (w^{T} x_{n} +b)\}\\
&=\frac{1}{2}\Vert w\Vert ^{2} +\sum ^{N}_{n=1} \alpha _{n} -\sum ^{N}_{n=1} \alpha _{n} y_{n} w^{T} x_{n} +\sum ^{N}_{n=1} \alpha _{n} y_{n} b\\
&=\frac{1}{2}\Vert w\Vert ^{2} +\sum ^{N}_{n=1} \alpha _{n} -\sum ^{N}_{n=1} \alpha _{n} y_{n} w^{T} x_{n}\\
&=\frac{1}{2}\sum ^{N}_{n=1} \alpha _{n} y_{n} x_{n}\sum ^{N}_{m=1} \alpha _{m} y_{n} x_{m} +\sum ^{N}_{n=1} \alpha _{n} -\sum ^{N}_{n=1} \alpha _{n} y_{n}\left(\sum ^{N}_{m=1} \alpha _{m} y_{m} x_{m}\right) x_{n}\\
&=\frac{1}{2}\sum ^{N}_{n=1}\sum ^{N}_{m=1} \alpha _{n} \alpha _{m} y_{n} y_{n} x_{n} x_{m} +\sum ^{N}_{n=1} \alpha _{n} -\sum ^{N}_{n=1}\sum ^{N}_{m=1} \alpha _{n} \alpha _{m} y_{n} y_{m} x_{n} x_{m}\\
&=\sum ^{N}_{n=1} \alpha _{n} -\frac{1}{2}\sum ^{N}_{n=1}\sum ^{N}_{m=1} \alpha _{n} \alpha _{m} y_{n} y_{n} x_{n} x_{m}
\end{aligned}
$$

其中限制条件为：

$$
\alpha_n\geq0,\ n=1,...,N\\
\sum_{n=1}^N\alpha _ny_n=0
$$

那么如何高效地解出这个式子呢？需要用到SMO算法，但是在介绍SMO之前，先来看下线性不可分的数据场景下，SVM要怎么处理。

#### 线性支持向量机

若数据线性不可分，或者存在一个样本点使得最终的结果泛化能力降低，那么可以增加松弛因子来解决这个问题。

约束条件则转化为：$$ y*(wx + b) \geq 1 - \xi$$

目标函数则转化为：$$min\ \frac{1}{2}\left \|w\right \|^2  + C\cdot\xi$$

这里的C为一个超参数，保证》0

实际上最终我们得到的松弛因子就是损失，前面的是1/2 \|w\| 2是一个L2的正则项

SVM存在的问题：

1. 训练样本不能太大，否则训练速度很慢
2. 
SVM的优点有：

1. SVM本身基本一定的防止过拟合的能力

SVM可以用来划分多分类吗？

可以，两个方法，一是直接多分类，时间复杂度是K方乘以一个SVM的时间复杂度和1 vs 1的分类器是一样的，二是使用1 vs rest / 1 vs 1

神经网络也可以处理非线性问题，为什么去全连接层换成SVM的效果会更好呢？在这个问题上SVM会有优势吗？

SVM会更好一点，因为它天然地加入了一点防止过拟合的东西，可能会好一些

一个softmax回归就是一个神经网络

为了减轻噪声样本对SVM的影响，那么就把C调小，使得泛化能力提高

## SMO算法

我们根据拉格朗日得到的对偶形式如下：

$$
arg\, {\underset {\alpha}{\operatorname {max}} } \sum_{i=1}^N\alpha_i - \frac{1}{2}\sum_{i=1}^N\sum_{j=1}^Ny_iy_j\alpha_i\alpha_j\langle x_i, x_j\rangle \\
s. t. \  \ \  \alpha_i \geq 0 \\
\sum_{i=1}^N\alpha_iy_i=0
$$

需要优化的参数就是$$\alpha = (\alpha_1, \alpha_2, ..., \alpha_N)$$，如果直接优化这么多参数的话，会使得计算变得非常复杂，所有我们需要找到其他方法来解决这个问题，这就是SMO算法。

SMO算法的思想是每次只选取一对$$(\alpha_i, \alpha_j)$$进行优化，根据上面的约束条件，其实选定了两个变量，实际上只有一个变量在变化，按照推导来说明。

假设我们现在选择的一对位$$(\alpha_1, \alpha_2)$$，那么根据约束条件可以得到如下的等式：

$$
\alpha_1y_1 + \alpha_2y_2 = -\sum_{i=3}^N\alpha_iy_i
$$

在进行一个化简，因为每次只优化两个参数，其余的参数都可以看成常量，所以令等式右边为一个常数$$\gamma$$:

$$
\alpha_1y_1 + \alpha_2y_2 = \gamma
$$

![](../.gitbook/assets/image%20%283%29.png)

然后我们按照固定的两个参数写出有优化的式子，将其他与$$\alpha_1，\alpha_2$$无关的常数项写成C：

$$
W(\alpha )\ =\sum ^{N}_{i=1} \alpha _{i} -\frac{1}{2}\sum ^{N}_{i=1}\sum ^{N}_{j=1} y_{i} y_{j} \alpha _{i} \alpha _{j} \langle x_{i} ,x_{j} \rangle \\
W(\alpha _{1} ,\alpha _{2} )=\alpha _{1} +\alpha _{2} -\frac{1}{2} K_{1,1} y^{2}_{1} \alpha ^{2}_{1} -\frac{1}{2} K_{2,2} y^{2}_{2} \alpha ^{2}_{2} -\frac{1}{2} K_{1,2} y_{1} y_{2} \alpha _{1} \alpha _{2} \ -\frac{1}{2} K_{2,1} y_{2} y_{1} \alpha _{2} \alpha _{1}\\
-y_{1} \alpha _{1}\sum ^{N}_{i=3} \alpha _{i} y_{i} K_{i,1} -y_{2} \alpha _{2}\sum ^{N}_{i=3} \alpha _{i} y_{i} K_{i,2} +C\\
=\ \alpha _{1} +\alpha _{2} -\frac{1}{2} K_{1,1} y^{2}_{1} \alpha ^{2}_{1} -\frac{1}{2} K_{2,2} y^{2}_{2} \alpha ^{2}_{2} -K_{1,2} y_{1} y_{2} \alpha _{1} \alpha _{2}\\
-y_{1} \alpha _{1}\sum ^{N}_{i=3} \alpha _{i} y_{i} K_{i,1} -y_{2} \alpha _{2}\sum ^{N}_{i=3} \alpha _{i} y_{i} K_{i,2} +C
$$

于是得到一个二元函数的优化。然后继续化简，由$$y_iy_i = 1$$，不管是正例负例二者相乘的结果为1，那么这个时候我们可以根据上面推到的式子中共两边同时乘以$$y_2$$得到：

$$
\alpha_1y_1y_2 + \alpha_2y_2y_2 = \gamma y_2 \\
\alpha_2=\gamma y_2 - \alpha_1y_1y_2
$$

带入$$\alpha_2$$ 的表达式，同时，令$$v_1 = \sum_{i=3}^N\alpha_iy_iK_{i,1} , v_2=\sum_{i=3}^N\alpha_iy_iK_{i,2}$$最终的得到：

$$
W(\alpha _{1} ,\alpha _{2} )=\ \gamma y_{1} -\alpha _{2} y_{1} y_{2} +\alpha _{2} -\frac{1}{2} K_{1,1} y^{2}_{1}( \gamma y_{1} -\alpha _{2} y_{1} y_{2})^{2} -\frac{1}{2} K_{2,2} y^{2}_{2} \alpha ^{2}_{2} -K_{1,2} y_{1} y_{2}( \gamma y_{1} -\alpha _{2} y_{1} y_{2}) \alpha _{2}\\
-y_{1}( \gamma y_{1} -\alpha _{2} y_{1} y_{2})\sum ^{N}_{i=3} \alpha _{i} y_{i} K_{i,1} -y_{2} \alpha _{2}\sum ^{N}_{i=3} \alpha _{i} y_{i} K_{i,2} +C\\
=\gamma y_{1} -\alpha _{2} y_{1} y_{2} +\alpha _{2} -\frac{1}{2} K_{1,1} y^{2}_{1}( \gamma y_{1} -\alpha _{2} y_{1} y_{2})^{2} -\frac{1}{2} K_{2,2} y^{2}_{2} \alpha ^{2}_{2} -K_{1,2} y_{1} y_{2}( \gamma y_{1} -\alpha _{2} y_{1} y_{2}) \alpha _{2}\\
-y_{1}( \gamma y_{1} -\alpha _{2} y_{1} y_{2}) v_{1} -y_{2} \alpha _{2} v_{2} +C
$$

然后我们的工作是进行求导：

$$
\begin{aligned}
\frac{\partial W(\alpha _{2} )}{\partial \alpha _{2}} & =-y_{1} y_{2} +1-K_{1,1} y^{2}_{1} (\gamma y_{1} -\alpha _{2} y_{1} y_{2} )y_{1} y_{2} -K_{2,2} y^{2}_{2} \alpha _{2} -K_{1,2} y_{1} y_{2} (\gamma y_{1} -\alpha _{2} y_{1} y_{2} )+K_{1,2} y^{2}_{1} y^{2}_{2} \alpha _{2} -y^{2}_{1} y_{2} v_{1} -y_{2} v_{2}\\
 & (带入y^{2}_{i} =1化简)\\
 & =-y_{1} y_{2} +1-K_{1,1} \gamma y_{2} -K_{1,1} \gamma \alpha _{2} -K_{2,2} \alpha _{2} -K_{1,2} \gamma y_{2} +K_{1,2} \alpha _{2} +K_{1,2} \alpha _{2} -y_{2} v_{1} -y_{2} v_{2}\\
 & (合并同类项规范式子)\\
 & =-(K_{1,1} +K_{2,2} -2K_{1,2} )\alpha _{2} -K_{1,2} \gamma y_{2} -K_{1,1} \gamma y_{2} -y_{2} (v_{1} -v_{2} )-y_{1} y_{2} +1\\
 & (这里非常重要，把1作为一个等量代换，1=y^{2}_{2})\\
 & =-(K_{1,1} +K_{2,2} -2K_{1,2} )\alpha _{2} -K_{1,2} \gamma y_{2} -K_{1,1} \gamma y_{2} -y_{2} (v_{1} -v_{2} )-y_{1} y_{2} +y^{2}_{2} = 0
\end{aligned}
$$

接下来继续进行化简，我们知道SVM对数据的预测最终要计算的表达是：$$f(x)=\sum\limits_{i=1}^N\alpha _iy_iK(x_i, x)+b$$，所以我们对$$v_1$$和$$v_2$$的值就可以进行一个代换：

$$
\begin{aligned}
v_{1} &=\sum ^{N}_{i=3} \alpha _{i} y_{i} K_{1,i} =f( x_{1}) -\alpha _{1} y_{1} K_{1,1} -\alpha _{2} y_{2} K_{1,2} -b\\
v_{2} &=\sum ^{N}_{i=3} \alpha _{i} y_{i} K_{2,i} =f( x_{2}) -\alpha _{1} y_{1} K_{2,1} -\alpha _{2} y_{2} K_{2,2} -b\\
v_{1} -v_{2} &=f( x_{1}) -f( x_{2}) -\alpha _{1} y_{1}( K_{1,1} -K_{1,2}) -\alpha _{2} y_{2}( K_{1,2} -K_{2,2})\\
&( 带入\alpha _{1} =( \gamma -\alpha _{2} y_{2}) y_{1})\\
&=f( x_{1}) -f( x_{2}) -( \gamma -\alpha _{2} y_{2}) y_{1} y_{1}( K_{1,1} -K_{1,2}) -\alpha _{2} y_{2}( K_{1,2} -K_{2,2})\\

&( 其中y_{1} y_{1} =y^{2}_{1} =1)\\
&=f( x_{1}) -f( x_{2}) -\gamma ( K_{1,1} -K_{1,2}) +\alpha _{2} y_{2}( K_{1,1} -K_{1,2}) -\alpha _{2} y_{2}( K_{1,2} -K_{2,2})\\
&( 其中K_{1,2} =K_{2,1})\\
&=f( x_{1}) -f( x_{2}) -\gamma ( K_{1,1} -K_{1,2}) +\alpha _{2} y_{2}( K_{1,1} +K_{2,2} -2K_{1,2})
\end{aligned}
$$

令预测值和真实值的误差为：$$E_i=f(x_i)-y_i$$，$$\eta=K_{1,1}+K_{2,2}-2K_{1,2}$$，同时带入$$v_1-v_2$$，得到：

$$
\begin{aligned}
\frac{\partial W(\alpha _{2} )}{\partial \alpha _{2}}& =-\eta \alpha ^{new}_{2} +K_{1,2} \gamma y_{2} -K_{1,1} \gamma y_{2} -y_{2} [f(x_{1} )-f(x_{2} )-\gamma (K_{1,1} -K_{1,2} )+\alpha ^{old}_{2} y_{2} \eta ]-y_{1} y_{2} +y^{2}_{2}\\
&=-\eta \alpha ^{new}_{2} +K_{1,2} \gamma y_{2} -K_{1,1} \gamma y_{2} -y_{2}[ f(x_{1} )-f(x_{2} )] +y_{2} \gamma (K_{1,1} -K_{1,2} )+\alpha ^{old}_{2} y^{2}_{2} \eta -y_{1} y_{2} +y^{2}_{2}\\
&=-\eta \alpha ^{new}_{2} +y_{2}[ f(x_{1} )-f(x_{2} )] +\alpha ^{old}_{2} y^{2}_{2} \eta -y_{1} y_{2} +y^{2}_{2}\\
&=-\eta \alpha ^{new}_{2} +\alpha ^{old}_{2} \eta +y_{2}\left( f(x_{1} )-f(x_{2} )+y_{2} -y_{1}\right)\\
&=-\eta \alpha ^{new}_{2} +\eta \alpha ^{old}_{2} +y_{2}\left(E_1 - E_2\right) = 0
\end{aligned}
$$

然后我们可以得到的是：

$$
\alpha ^{new}_{2} =\alpha ^{old}_{2} +\frac{y_{2}( E_{1} -E_{2})}{\eta }
$$

#### 下一步我们要对原始解进行裁剪

为什么需要裁剪呢？是因为我们的计算得到的原始解可能会超出上面的限制条件规定的范围：

$$
\alpha_1y_1 + \alpha_2y_2 = -\sum_{i=3}^N\alpha_iy_i = \gamma\\
0\leq\alpha_i \leq C
$$

![](../.gitbook/assets/image%20%287%29.png)

左图这边，我们可以的看到，此时$$y_1\ne y_2$$，此时的限制条件就可以写成$$\alpha_1-\alpha_2=k$$，根据$$k$$的正负可以得到不同的上下界，因此可以写成：

下界：$$L=max(0, \alpha_2^{old}-\alpha_1^{old})$$

上界：$$H=max(C, C+\alpha_2^{old}-\alpha_1^{old})$$

同样的根据右图的内容，此时$$y_1= y_2$$，此时的限制条件就可以写成$$\alpha_1+\alpha_2=k$$:

下界：$$L=max(0, \alpha_2^{old}+\alpha_1^{old}-C)$$

上界：$$H=max(C, \alpha_2^{old}+\alpha_1^{old})$$

根据上面的上下界可以得到：

$$
\alpha_{2}^{n e w}=\left\{\begin{array}{ll}
H & \alpha_{2}^{\text {new,unclipped}}>H \\
\alpha_{2}^{\text {new,unclipped}} & L \leq \alpha_{2}^{\text {new, unclipped}} \leq H \\
L & \alpha_{2}^{\text {new,unclipped}}<L
\end{array}\right.
$$

然后根据$$\alpha_1^{old}y_1+\alpha_2^{old}y_2=\alpha_1^{new}y_1+\alpha_2^{new}y_2$$得到：

$$
\alpha_1^{new}=\alpha_1^{old}+y_1y_2(\alpha_2^{old}-\alpha_2^{new})
$$

到这里我们就可以选取一对$$\alpha_i, \alpha_j$$进行优化更新了。

### 更新阈值b

当$$0<\alpha _1 ^{new} < C$$，满足$$y_1(w^T+b)=1$$,此时我们两边同时乘以$$y_1$$可以得到$$\sum \limits ^N _{i=1}\alpha_iy_iK_{i,1}=y_1$$进而可以得到$$b_1^{new}$$的值：

$$
b_1^{new}=y_1- \sum_{i=1}^N\alpha_iy_iK_{i,1}-\alpha_1^{new}y_1K_{1,1}-\alpha_2^{new}y_2K_{2,1}
$$

其中上式中的前两项可以写成：

$$
y_{1}-\sum_{i=3}^{N} \alpha_{i} y_{i} K_{i, 1}=-E_{1}+\alpha_{1}^{\text {old }} y_{1} K_{1,1}+\alpha_{2}^{\text {old }} y_{2} K_{2,1}+b^{\text {old }}
$$

当$$0<\alpha_2^{new}<C$$，可以得到：

$$
b_{2}^{\text {new}}=-E_{2}-y_{1} K_{1,2}\left(\alpha_{1}^{\text {new}}-\alpha_{1}^{\text {old}}\right)-y_{2} K_{2,2}\left(\alpha_{2}^{\text {new}}-\alpha_{2}^{\text {old}}\right)+b^{\text {old}}
$$

当$$b_1$$和$$b_2$$都有效的时候他们是相等的，即$$b^{new}=b_1^{new}=b_2^{new}$$

当$$\alpha_1, \alpha_2$$都在边界上的时候，且$$L\ne H$$时，$$b_1,b_2$$之间的值就是和KKT条件一致的阈值。SMO算法选择他们的中点作为新的阈值：

$$
b^{new}=\frac{b_1^{new}+b_2^{new}}{2}
$$

至此，关于SVM所需要的参数我们都可以计算出来了。





### 参考文献

* \[1\]邹博.机器学习升级版.小象学院，2018.
* \[2\]秦曾昌.机器学习算法精讲.小象学院，2018.
* \[3\]李航.统计学习方法\[M\].北京：清华大学出版社，2012.
* \[4\]化学狗码砖的日常.机器学习算法实践-SVM中的SMO算法.[知乎](https://zhuanlan.zhihu.com/p/29212107)
* \[5\]Bishop.Pattern Recognition And Machine Learning，



