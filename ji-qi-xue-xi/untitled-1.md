# 机器学习中的分类指标

对于机器学习中的分类问题的结果，需要某种指标来衡量这个分类器的性能。最简单的就是直接比较预测值和真实值，得到一个准确率，在**scikit-learn**中，使用的是`accuracy_score`函数来实现。但是呢，这个指标不够满足现实中的需求，所以又有了准确率和召回率，以及F1分数和F-beta分数几个指标，这里总结在此方便查阅。

全文代码所用依赖和测试用例如下：（为了页面的查阅方便，所以写成了很多行，实际代码中可以尽量写在一行中）

其中`y_true`

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
print
```















