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
/*
 例子：求1071 和462的最大公约数
 1071 = 2 X 462 + 147   a = 1071, b = 462
 462 = 3 X 147 + 21     a = 462 , b = 147
 147 = 7 X 21 + 0       a = 147 , b = 21
                        a = 21  , b = 0
 */
long long gcd (long long a, long long b) {
    return (b == 0)? abs(a) : gcd(b, a % b);
}
```

  素数判断

```c
bool isPrime(int n) {
    if (n <= 1) return false;
    int sqr = int(sqrt(n * 1.0));
    for (int i = 2; i <= sqr; i++) {
        if (n % i == 0)
            return false;
    }
    return true;
}
```

闰年判断

```c
bool isLeap(int year) {
    // 能被4整除但不能被100整除，或者是能被400整除的
    return (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
}
```

进制转换：







## 参考材料

1. [柳婼大神的PAT github](https://github.com/liuchuo/PAT)

