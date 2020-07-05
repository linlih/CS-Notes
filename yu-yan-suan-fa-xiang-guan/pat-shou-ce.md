# PAT手册



## 树

 二叉树的遍历：前序，中序，后序  1086.Tree Traversals Again \(25分\)

## 图 

dfs+dijkstra 1087.All Roads Lead to Rome \(30分\)



Dijkstra算法：

```c
#include <i
```

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

### 冒泡排序

```c
void bubbleSort(int arr[], int n) {
    bool NoSwap;
    for (int i = 0; i < n - 1; ++i) {
        NoSwap = true;
        for (int j = n - 1; j > i; --j) {
            if (arr[j] < arr[j - 1]) {
                swap(arr[j], arr[j - 1]);
                NoSwap = false;
            }
        }
        if (NoSwap) return;
    }
}
```

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
        swap(arr[i], arr[k]);
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

```c
void quickSort(int arr[], int left, int right) {
    if (right <= left) return;
    int pivot = selectPivot(left, right);
    swap(arr, pivot, right);
    pivot = partion(arr, left, right);
    quickSort(arr, left, pivot - 1);
    quickSort(arr, pivot + 1, right);
}

int selectPivot(int left, int right) {
    return (left + right)/2;
}

int partion(int arr[], int left, int right) {
    int l = left;
    int r = right;
    int temp = arr[right];
    while(l != r) {
        while(arr[l] <= temp && r > l)
            l++;
        if (l < r) {
            arr[r] = arr[l];
            r--;
        }
        while(arr[r] >= temp && r > l)
            r--;
        if (l < r) {
            arr[l] = arr[r];
            l++
        }
    }
    arr[l] = temp;
    return l;
}
```

### 归并排序

```c

```



考察的题型有：

| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1 | 判断是否是插入排序，归并排序的序列 | [1089.Insert or Merge \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805377432928256) |
| 2 | 判断是否是插入排序，堆排序的序列 | [1098.Insertion or Heap Sort \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805368847187968) |

这两道题都是给定了两个序列，一个是未排序初始序列，另一个是排序为完成的序列，判断是采用哪种排序方式，然后给出下一轮次的排序结果。

判断是否是插入排序的方法：

```c
/**
 a[]为初始序列，b[]为未完成排序序列，n为序列元素个数
 例子如下：n = 10
 3 1 2 8 7 5 9 4 6 0
 1 2 3 7 8 5 9 4 6 0
 判断方法是很trivial的，首先插入排序的特定是每次排序一个元素，并且是从右往左开始依次排序
 得到的序列结果是已排序部分是有序的，未排序部分是没有进行任何操作的
 */
for (int i = 0; i < n - 1 && b[i] <= b[i+1]; ++i); // 找到已排序和未排序的分界点
for (int j = i + 1; a[j] == b[j] && j < n; ++j);   // 未排序部分没有操作，固和原序列一致
if (j == n) {
    cout << "Insertion Sort" << endl;
}
// 然后要输出下一轮次的，只需要排序前面的i+1个元素即可，直接调用sort函数
// 注意，sort排序是前闭后开区间，所以是i+2
// 对比排序vector: sort(v.bein(), v.end())，v.end()指向的是容器最后一个元素的下一个元素
sort(a, a + i + 2); 
```

这两道题可以选择取巧的方式，因为只判断两种排序方式，不是其中一种，那就一定是另外一种了，所以只需要判断一种排序方式即可，相比之下，插入排序的判断方式是比较快的，所以只要判断是否是插入排序，不是的话一定就是另外一种排序方式了。



| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1 | 判断序列元素是否可以作为pivot | [1101.Quick Sort \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805366343188480) |

判断的思想是这样，一个元素能够作为pivot的条件是，序列的左半部分比pivot小，右半部分比pivot大，关键点在于，这个选为pivot的数值在一轮排序中是处于最后位置的，这个是这道题的核心。如果直接判断左半部分和右半部分满不满足条件的话，测试用例会超时，因为这样是$$ O(n^2)$$的时间复杂度。

这道题的其他解法，参考： [DedicateToA 的这篇文章](https://blog.csdn.net/DedicateToAI/article/details/102680110)。



## 深度优先搜索和广度优先搜索

| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1 | 供应链所有价格 | [1079.Total Sales of Supply Chain \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805388447170560) |
| 2 | 供应链最大价格 | [1090.Highest Price in Supply Chain \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805376476626944) |
| 3 | 供应链最小价格 | [1106.Lowest Price in Supply Chain \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805362341822464) |

这三道题的类型一致，都是深度优先搜索，只不过根据题目的要求不同判断条件不同，万变不离其宗，可以对比着学习。

## 有趣的算法问题

给定一个数组为1~N，如果输出它的全排列呢？（《算法笔记》P115）



## 注意！！

1. 使用scanf读入字符时要注意，前面加个空格，原因[参考这里](https://blog.csdn.net/weixin_46368810/article/details/105867661)

```c
// 输入为：1 2
// 这样才可以正确读取到内容
scanf(" %c %c", &a, &b);
```

## 参考材料

1. [柳婼大神的PAT github](https://github.com/liuchuo/PAT)
2. 《算法笔记》胡凡.曾磊.著

