# ML问题总结

## matplotlib总结

#### 显示中文需要添加：

```python
import matplotlib as plt
plt.rcParams['font.sas-serig']=['SimHei'] # 用来正确显示中文标签
plt.rcParams['axes.unicode_minus']=False # 用来争取显示正负号
```

## Numpy总结

np.r是按列连接两个矩阵，就是把两矩阵上下相加，要求列数相等，类似于pandas中的concat\(\)。 np.c是按行连接两个矩阵，就是把两矩阵左右相加，要求行数相等，类似于pandas中的merge\(\)。

### stack, vstack, hstack

ref: [https://cloud.tencent.com/developer/article/1378491](https://cloud.tencent.com/developer/article/1378491)

### range和np.arange的区别

ref: [https://blog.csdn.net/lanchunhui/article/details/49493633](https://blog.csdn.net/lanchunhui/article/details/49493633)



## Pandas

使用read\_csv读入文件的时候，访问的方式不同得到的变量的类型不同

```python
import pandas ad pd
data = pd.read_csv('train.csv')
data[['occupation']] # 返回的格式是：pandas.core.series.Series
data['occupation']   # 返回的格式是：pandas.core.frame.DataFrame
```



## 打印juypter运行时间

magic函数 magic有行魔法%time 和单元魔法%%time

