# PAT手册



## 树

 二叉树的遍历：前序，中序，后序  1086.Tree Traversals Again \(25分\)

## 图 

dfs+dijkstra 1087.All Roads Lead to Rome \(30分\)

## 算术运算 

这两道题的核心是最大公约数的求法，使用辗转相除法 

| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1 | 有理数分数求和 | 1081.Rational Sum \(20分\) |
| 2 | 有理数分数四则运算  | 1088.Rational Arithmetic \(20分\)  |

辗转相除法：

```c
long long gcd (long long a, long long b) {
    return (b == 0)? abs(a) : gcd(b, a % b);
}
```

  







## 参考材料

1. [柳婼大神的PAT github](https://github.com/liuchuo/PAT)

