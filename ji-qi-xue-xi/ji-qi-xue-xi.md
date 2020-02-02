# 机器学习中的分类指标

对于机器学习中的分类问题的结果，需要某种指标来衡量这个分类器的性能。最简单的就是直接比较预测值和真实值，得到一个准确率，在**scikit-learn**中，使用的是`accuracy_score`函数来实现。但是呢，这个指标不够满足现实中的需求，所以又有了准确率和召回率，以及F1分数和F-beta分数几个指标，这里总结在此方便查阅。

全文代码所用依赖和测试用例如下：（为了页面的查阅方便，所以写成了很多行，实际代码中可以尽量写在一行中）

其中`y_true`表示的真实值，`y_hat`表示的是预测值。

```python
import numpy as np
from sklearn.metrics import accuracy_score
from sklearn.metrics import precision_score, recall_score
from sklearn.metrics import f1_score, fbeta_score
from sklearn.metrics import precision_recall_fscore_support
from sklearn.metrics import classification_report

y_true = np.array([1, 1, 1, 1, 0, 0])
y_hat  = np.array([1, 0, 1, 1, 1, 1])
```

### 正确率

这个属于最简单的性能衡量指标，思想也非常简单，直接比较预测值和真实值的内容是否相同，实例如下：

```python
# 总共6个样本，3个正确，3个错误
print(accuracy_score(y_true, y_hat)) # 0.5错误
```

下面的指标需要用到几个参数，这几个词汇怎么理解呢，首先true和false表示的是真实情况与预测情况是否一致，比如说真实值为1，预测值为1，那么true就为1。positive和negtive指的是预测的结果，为正例则为1，负例为0。

> tp : true positive，表示预测结果为正例的正确数量  
> fp : false positive，表示预测结果为正例的错误数量  
> tn : true negtive，表示预测结果为负例的正确数量  
> fn : false negtive，表示预测结果为负例的错误数量

### 查准率

查准率从字面上也很好理解，就是保证分类器挑出的正例的正确性，简单说就是衡量分类器选择的商品一定是好商品的能力。

计算公式如下：

$$
precision = \frac{tp}{tp + fp}
$$

```python
# 预测结果为正例的样本数共有5个，正确的数量有3个，即tp = 3, fp = 2
print(precision_score(y_true, y_hat)) # 3 / ( 3 + 2) = 0.6
```

### 查全率

同样的查全率也和字面意思相同，衡量的是分类器是否挑出了所有样本中正例的能力。简单说就是100件商品中，有50件好的商品，该参数就是衡量分类器能不能把所有的好商品选出来的能力。

计算公式如下：

$$
recall = \frac{tp}{tp + fn}
$$

```python
# tp为预测结果为正例的正确数量，为3个
# fn为预测结果为负例的错误数量，为1个
print(recall_score(y_true, y_hat)) # 3 / (1 + 3) = 0.75
```

### F1 score

计算公式如下：

$$
F1 = 2 * \frac{precision * recall}{precision + recall}
$$

```python
print(f1_score(y_true, y_hat)) 
# (3/5) * (3/4) / (3/5 + 3/4) = 0.6666666
```

### F-beta score

该指标和`F1 score`一样都是`precision`和`recall`两个参数的加权调和平均数，`F1 score`是`F-beta score`中`beta`等于1的特殊情况。

计算公式如下：

$$
F_\beta = \frac{(1 + \beta^2)*precision*recall}{(\beta^2*precision) + recall}
$$

 从公式上我们也可以看出，当 $$\beta$$ 等于0时，$$F_\beta$$的值就是`precision`，当$$\beta$$趋向无穷大的时候，$$F_\beta$$的值就是`recall`。

补充F1 score和F-beta score二者调和平均定义的书写方式：

$$
\begin{aligned}
\frac{1}{F1} &= \frac{1}{2}*(\frac{1}{precision} + \frac{1}{recall})
\\
\frac{1}{F_\beta} &= \frac{1}{1+\beta^2} * (\frac{1}{precision } + \frac{\beta^2}{recall})
\end{aligned}
$$

```python
print('F-beta: ')
for beta in np.logspace(-3, 3, num=7, base=10):
  fbeta = fbeta_score(y_true, y_hat, beta=beta)
  print('\tbeta=%9.3f\tF-beta=%.5f' % (beta, fbeta))

# 输出结果如下：precision = 0.6, recall = 0.75
'''
F-beta: 
	beta=    0.001	F-beta=0.60000
	beta=    0.010	F-beta=0.60001
	beta=    0.100	F-beta=0.60119
	beta=    1.000	F-beta=0.66667
	beta=   10.000	F-beta=0.74815
	beta=  100.000	F-beta=0.74998
	beta= 1000.000	F-beta=0.75000
'''
```



