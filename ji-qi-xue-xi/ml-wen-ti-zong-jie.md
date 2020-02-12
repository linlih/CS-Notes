# ML问题总结

## matplotlib总结

#### 显示中文需要添加：

```python
import matplotlib as plt
plt.rcParams['font.sas-serig']=['SimHei'] # 用来正确显示中文标签
plt.rcParams['axes.unicode_minus']=False # 用来争取显示正负号
```

## Numpy总结

np.r_是按列连接两个矩阵，就是把两矩阵上下相加，要求列数相等，类似于pandas中的concat\(\)。 np.c_是按行连接两个矩阵，就是把两矩阵左右相加，要求行数相等，类似于pandas中的merge\(\)。
