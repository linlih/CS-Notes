# 频率派和贝叶斯学派

频率派的角度是从说$$\theta$$是未知的，但是确定的，可以通过频率来近似。那么在实际的应用中常常使用ML（Maximum Likelihood Estimation 最大似然估计）的方法来求解。

频率学派存在一定的局限性，比如说无法评估不可重复实验的事件发生概率，比如说冰川消失的概率，这个事件是无法实验的，也就无法通过独立重复实验的频率来近似得到事件的概率了。

贝叶斯学派则认为参数$$\theta$$是一个随机变量，服从于一个先验分布，所以如何确定这个先验分布是一个难点。在实际实现中常常使用MAP（Maximum a posterior probability最大后验概率）。那么这个也是比较好理解的，贝叶斯首先假定了一个参数的分布，之所以叫做先验，是说我们在未观测数据之前就假定的，根据是人过往的经验来确定的。那么后验概率的意思就是说，当我拿到了新的数据之后，我对之前假定的先验分布进行调整使得这个分布接近于真实的分布，从而得到正确的结果。





公式回顾：

全概率公式：

事件B的所有可能结果为：B1，B2，~Bn，并且两两之间相互独立。

$$
P(A)=P(A \cap \Omega) = P[A\cap(B_1\cup B_2 ... \cup B_n)] \\=P(AB_1\cup AB_2...\cup AB_n) \\ = P(A B_1)+P(AB_2)+ ...+P(AB_n)\\=P(A|B_1)P(B_1)+P(A|B_2)P(B_2)+...P(A|B_n)P(B_n)
\\=\sum_i^nP(B_i)P(A|B_i)
$$





参考内容：

{% embed url="https://blog.csdn.net/lsldd/article/details/84488678?utm\_medium=distribute.pc\_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel\_param&depth\_1-utm\_source=distribute.pc\_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel\_param" %}



