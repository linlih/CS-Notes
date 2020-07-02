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

```c
// 将sum转换成d进制
int ans[31], num = 0;
do {
    ans[num++] = sum % d;
    sum /= d;
} while(sum!= 0);
```



## 排序

关于排序，基本排序算法基本不会让你自己实现，所以对于选择排序，插入排序，快速排序这些，只需了解基本思想，能够知道实现流程即可。可能考察的题型，是链表的排序，就需要用到选择排序，或者插入排序的思想，因为链表不能直接跳转，无法直接使用sort函数。

### 选择排序

```c
void selecetSort(int arr[], int n) {
    for (int i = 0; i < n-1; ++i) {
        int k = i;
        // 找到序列中未排序的最小元素
        for (int j = i+1; j < n; ++j) {
            if (arr[j] < arr[i])
                k = j;
        }
        swap(arr[i], arr[k];
    }
}
```

### 插入排序

```c
void insertSort(int arr[], int n) {
    for (int i = 1; i < n; ++i) {
        int temp = arr[i], j = i;
        // 找到已排序中的插入位置
        while(j > 0 && temp < arr[j]) {
            arr[j] = arr[j-1];
            j--;
        }
        arr[j] = temp;
    }
}
```

### 快速排序



考察的题型有：

| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1 | 判断是否是插入排序，归并排序的序列 | [1089.Insert or Merge \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805377432928256) |
| 2 | 判断是否是插入排序，堆排序的序列 | [1098.Insertion or Heap Sort \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805368847187968) |

这两道题都是给定了两个序列，一个是未排序初始序列，另一个是排序为完成的序列，判断是采用哪种排序方式，然后给出下一轮次的排序结果。

判断是否是插入排序的方法：

```c

```



## 深度优先搜索和广度优先搜索







## 参考材料

1. [柳婼大神的PAT github](https://github.com/liuchuo/PAT)

