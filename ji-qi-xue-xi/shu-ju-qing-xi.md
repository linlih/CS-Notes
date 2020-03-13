# 数据清洗

### 缺失值处理

针对sklearn 0.22.x版本与之前的版本不太一样，0.16.x是放在preprocessing模块下，而**0.22.x版本则是放在了impute模块下面**，要注意！

ref: [https://scikit-learn.org/stable/modules/impute.html](https://scikit-learn.org/stable/modules/impute.html)

```python
# 处理数值类型的
import numpy as np
from sklearn.impute import SimpleImputer
imp = SimpleImputer(missing_values=np.nan, strategy='mean')
imp.fit([[1, 2], [np.nan, 3], [7, 6]])
X = [[np.nan, 2], [6, np.nan], [7, 6]]
print(imp.transform(X))
"""
[[4.          2.        ]
 [6.          3.666...]
 [7.          6.        ]]
"""
```

```python
# 处理类别类型的
import pandas as pd
df = pd.DataFrame([["a", "x"],
                   [np.nan, "y"],
                   ["a", np.nan],
                   ["b", "y"]], dtype="category")

imp = SimpleImputer(strategy="most_frequent")
print(imp.fit_transform(df))
""" output:
[['a' 'x']
 ['a' 'y']
 ['a' 'y']
 ['b' 'y']]
"""
```





