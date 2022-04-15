# 背包问题 - 01背包&完全背包

## 问题定义

首先先定义下背包问题，方便后面讲述： 有N件物品，W容量的背包，第 i 件物品的重量为 weight\[i]，价值为 value\[i]，问在能装下的前提下如何使得背包的物品价值最大。

具体的例子如下，背包的容量为4。显然最大的价值是35。

| 物品编号 | 重量 | 价值 |
| ---- | -- | -- |
| 0    | 1  | 15 |
| 1    | 2  | 20 |
| 2    | 4  | 30 |

### 核心代码

首先回顾下两个核心代码：

0/1背包的核心代码：

```cpp
for (int i = 0; i < weight.size(); i++) {
	for (int j = bagWeight; j >= weight[i]; j--) {
		dp[j] = max(dp[j], d[j-weight[i]] + value[i]);
	}
}
```

完全背包的核心代码：

```cpp
for (int i = 0; i < weight.size(); i++) {
	for (int j = weight[i]; j <= bagWeght; j++) {
		dp[j] = max(dp[j], dp[j-weight[i]] + value[i]);
	}
}
```

可以看到这两个问题的差别只有内层for循环的顺序不同：

```cpp
for (int j = bagWeight; j >= weight[i]; j--) // 0/1背包，使用的是逆序遍历
	dp[j] = max(dp[j], d[j-weight[i]] + value[i]);
	
for (int j = weight[i]; j <= bagWeight; j++) // 完全背包，使用的是正序遍历
	dp[j] = max(dp[j], dp[j-weight[i]] + value[i]);
```

### 为什么调整遍历的方向就可以？

按照最经典的例子，来看下每次更新的dp是怎样的，dp的大小应该是容量的大小+1，也就是5。这里要注意，很多时候我们是会多申请一个空间，dp\[0]作为起始条件来推导使用。

对于0/1背包而言，当 `i=0` 遍历第一个物品时:

第一次遍历的过程：j=bagWeight=4，此时的条件是weight\[i] = weight\[0] = 1，dp\[4]=max(dp\[4], dp\[4-1]+value\[1]) = 15，结果如下：

![](<../.gitbook/assets/image (30).png>)

第二次遍历的过程：j = 3，weight\[i] = weight\[0] = 1，dp\[3] = max(dp\[3], dp\[3-1]+value\[1]) = 15，结果如下:

![](<../.gitbook/assets/image (29).png>)

遍历完第一个物品得到的结果为：

![](<../.gitbook/assets/image (31).png>)

对于完全背包而言，当 `i=0` 遍历第一个物品时: 第一次遍历的过程：j=weight\[i] = weight\[0] = 1，条件是 bagWeight=4，dp\[1] = max(dp\[1], dp\[1-1]+value\[0]) = 15，结果如下：

![](<../.gitbook/assets/image (34).png>)

第二次遍历的过程：j = 2，weight\[i] = weight\[0] = 1， dp\[2] = max(dp\[2], dp\[2-1]+value\[0]) = max(dp\[2], dp\[1]+value\[0]) = 30，结果如下：

![](<../.gitbook/assets/image (33).png>)

遍历完第一个物品得到的结果为：

![](<../.gitbook/assets/image (32).png>)

回顾下在0/1背包问题和完全背包问题中dp的定义：

dp\[j] 表示的是容量为 j 的背包所背物品的最大价值 -> 这个定义在两个问题中都是一致的。

通过上面的一个物品的遍历过程，可以看出两个遍历顺序带来的不同，在两个问题中是都需要利用之前的 dp 结果，也就是说dp\[j] 需要用到dp\[0...j-1]的结果，但是在完全背包问题中这些结果是已经计算过的，也就是说dp\[0...j-1]已经放入了0个或多个物品了。而在0/1问题中，这些结果还没有计算，也就是没有放入物品的时候，每当计算到dp\[i]的时候就只会放入一个物品。

## 应用举例

总结起来有三类问题，这里的核心代码根据题目会有一定的变通。遍历顺序只需要判断题目中"物品"能不能取多个来判断，多个就正向遍历，一个就逆向遍历。

解决问题的核心要点就是明确dp的定义，确定好初始化条件，然后明确遍历顺序，最后搞定！

其中遍历顺序上求的是组合数，就是外循环物品，内循环目标；如果是排列数，那就是外循环目标，内循环物品。

* 判断是否 核心代码

```cpp
dp[j] = dp[j] || dp[j-nums[i]]; 
```

* 统计个数 核心代码

```cpp
dp[j] += dp[j - nums[i]]; 
```

* 最值问题 核心代码

```cpp
dp[i] = min(dp[i], dp[i - j*j]+1);
dp[i] = max(dp[i], dp[i - j*j]+1);	
```

### 0/1背包

#### [416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/) -> 判断是否

题目说明：将一个数组分割成两个和相同的子集，返回当前这个数组是否可以满足这个要求。

例子：\[1, 5, 11, 5] 返回 true，因为可以分成 \[1, 5, 5]和\[11]

题目转化为0/1背包问题，就是在所有的数值中选，把数值当成物品，对应的数值大小就是value的大小，每个只能选一次，最终看能否选出和为所有数值总和一半的子集。 实现过程：

```cpp
// 这种实现方法有点不好理解
bool canPartition(vector<int>& nums) {
	int sum = 0;
	vector<int> dp(10001, 0); // dp[i]表示用元素构成背包容量为i的总和
	for (int i = 0; i < nums.size(); i++) {
		sum += nums[i];
	}
	// 如果和为奇数，那么显然不能分成两个和相等的子集，题目要求为正整数
	if (sum % 2 == 1) return false;
	int target = sum / 2;

	for (int i = 0; i < nums.size(); i++) {
		for (int j = target; j >= nums[i]; j--) {
			dp[j] = max(dp[j], dp[j-nums[i]]+nums[i]); 
		}
	}
	return dp[target] == target;
}

bool canPartition(vector<int>& nums) {
	int sum = 0;
	for (int i = 0; i < nums.size(); i++) {
		sum += nums[i];
	}
	if (sum % 2 == 1) return false;
	int target = sum / 2;
	vector<bool> dp(target+1, false); // dp[i]表示的是i数值能否被nums中的数值构成

	dp[0] = true; // 作为初始条件很重要，它的含义这里可以理解成数值0可以由任意的nums数组构成
	for (int i = 0; i < nums.size(); i++) {
		for (int j = target; j >= nums[i]; j--) {
			dp[j] = dp[j] || dp[j-nums[i]]; 
		}
	}
	return dp[target];
}
```

#### [494. 目标和](https://leetcode-cn.com/problems/target-sum/) -> 统计个数

题目说明：给定一个数组，可以在数值前面添上+号或者-号，构成target的方式多少种？

例子：\[1, 1, 1]，target = 1，那么就有三种构成方法，分别是 \[-1, 1, 1] \[1, -1, 1] \[1, 1, -1]

这道题第一眼看起来可能会想到回溯方法来做，转化成背包问题需要一点技巧。

首先我们将数值的和分成两部分，这个时候是没有加符号的，两个部分和的大小由加入的数值决定。只是将两个数值和分成两部分。

现在给pos部分加上+号，neg部分加上-号，那么我们可以构建出以下等式：

$$
\begin{aligned} pos - neg &= target\\ pos + neg &= sum \end{aligned}
$$

消掉 `pos`后得到：

$$
neg = \frac{sum-target}{2}
$$

其中，sum和target都是已知的，那么neg也就是已知的，当然如果要转换成求pos也是可以的。

转化成背包问题就是从nums中选择数字，放进大小为neg的背包中。

```cpp
int findTargetSumWays(vector<int>& nums, int target) {
	int sum = 0;
	for (int i = 0; i < nums.size(); i++) {
		sum += nums[i];
	}
	int diff = sum - target;
	if (diff < 0 || diff % 2 == 1) return 0;
	int neg = diff / 2;
	vector<int> dp(neg + 1, 0); // dp[j]表示的是j数值可以被nums构成的种类
	dp[0] = 1;  // 初始化条件，可以构成0大小的记为1次
	for (int i = 0; i < nums.size(); i++) {
		for (int j = neg; j >= nums[i]; j--) {
			dp[j] += dp[j - nums[i]]; 
		}
	}
	return dp[neg];
}
```

#### [474. 一和零](https://leetcode-cn.com/problems/ones-and-zeroes/) -> 最值问题

题目说明：给定一个0和1字符串的数组，找出满足m个0，n个1的最大子集长度。

举例说明：strs = \["10", "0", "1"]， m = 1，n = 1。那么结果为1，因为满足的子集为\["10"] 和 \["0"]，\["1"]，所以最大长度为2。

这道题就比较不一样了，不完全是相同的0/1背包的套路了。这道题是两个维度的0/1背包了。

```cpp
int findMaxForm(vector<string>& strs, int m, int n) {
	vector<vector<int>> dp(m+1, vector<int>(n+1, 0)); // dp[i][j]表示的i个0和j个1构成的最大子集长度
	for (const auto& str : strs) { // str相当于物品
		int numZeros = 0, numOnes = 0;
		for (const auto& c : str) {
			if (c == '0') numZeros++;
			else numOnes++;
		}
		for (int i = m; i >= numZeros; i--) {    // 1维 0/1背包 容量为m
			for (int j = n; j >= numOnes; j--) { // 2维 0/1背包 容量为n
				dp[i][j] = max(dp[i][j], dp[i-numZeros][j-numOnes] + 1);
			}
		}
	}
	return dp[m][n];
}
```

### 完全背包

#### [518. 零钱兑换 II](https://leetcode-cn.com/problems/coin-change-2/) -> 统计个数

题目说明：给定金额大小，和一定数量的零钱，问构成零钱金额共有多少种方式，每种零钱可以取多个。

举例说明：amount = 5, coins = \[1, 2, 5]，那么共有四种方案构成总金额，5 = 5； 5 = 2 + 2 + 1；5 = 2 + 1 + 1 + 1；5 = 1 + 1 + 1 + 1 + 1；

所以思路也是比较明确的和[494. 目标和](https://leetcode-cn.com/problems/target-sum/)是一样的，只不过按照前面的分析，0/1背包和完全背包的不同，只需要把内循环的逆向遍历改成正向遍历就可以了。

```cpp
int change(int amount, vector<int>& coins) {
	vector<int> dp(amount+1, 0);
	dp[0] = 1;
	for (int i = 0; i < coins.size(); i++) {
		// 目标和题目中是这么写的了，注意对比两者的区别
		// for (int j = amount; j >= coins[i]; j--)
		for (int j = coins[i]; j <= amount; j++) {
			dp[j] += dp[j-coins[i]];
		}
	}
	return dp[amount];
}
```

#### [377. 组合总和 Ⅳ](https://leetcode-cn.com/problems/combination-sum-iv/) -> 统计个数

题目说明：和零钱兑换是一样的，只不过这里的构成方案有顺序要求。

举例说明：target = 4, nums = \[1, 2, 3]，构成target的共有7中方案：4 = 1 + 1 + 1 + 1; 4 = 1 + 1 + 2; 4 = 1 + 2 + 1; 4 = 2 + 1 + 1; 4 = 2 + 2; 4 = 1 + 2; 4 = 3 + 1;

可以看得出，这个时候组合的方式有了顺序的要求，也就是说\[1, 2, 1] 和 \[2, 1, 1]是不同的组合方式，而在零钱兑换这两种方式是一样的。

```cpp
// 这个解法中有两个点需要注意
// 1. 官方题解中dp是用int来表示的，但是由于中间结果会溢出，所以在内层循环判断的时候加了dp[j] < INT_MAX - dp[j-nums[i]]的判断，但是要知道加了这个判断，算出来的dp结果是错误的！这一点很重要，只不过刚好这个结果又是对的，所以看起来这个判断是没有影响的。但是其实是有的，大家可以把对应溢出测试用例的dp数组打印出来对比下就知道了。去掉这个判断就是将int改成unsigned int就可以了。
// 2. 需要对比零钱兑换II的遍历方式，体会两者的差异
int combinationSum4(vector<int>& nums, int target) {
    vector<unsigned int> dp(target + 1, 0); // 如果是int类型中间结果会溢出
    dp[0] = 1;
    /* 零钱兑换II的遍历方式
    for (int i = 0; i <= nums.size(); i++)
	    for (int j = nums[i]; j <= target; j++)
    */
    for (int j = 1; j <= target; j++) {
 	   for (int i = 0; i < nums.size(); i++) {
 		   //if (j >= nums[i] && dp[j] < INT_MAX - dp[j-nums[i]]) { 
 		   if (j >= nums[i]) {
 			   dp[j] += dp[j-nums[i]];
 		   }
 	   }
    }
    return dp[target];
 }
```

可以看到和零钱兑换II的遍历方式只是内外层循环调了个个而已，最终得到的一个是组合数，一个是排列数。怎么理解呢？

首先先假定nums=\[1, 2, 3]，target=4。

然后我们来看零钱兑换得到的组合数，外层循环遍历的是nums的内容，也就是说，每个nums\[i]数值都只会进行遍历一次，也就是说对于某个target来说，只会选择一次该数值nums\[i]，所以最终得到的结果一定是按照nums\[i]遍历顺序得到的结果，也就说会得到\[1, 1, 2]的组合结果，而不会出现\[2, 1, 1]的组合结果，因为外层遍历是从nums\[0]到nums\[2]遍历的。

相应的，在组合遍历的过程中，是将nums的遍历移到内层循环中去了。所以对于每个target来说，那么它都有可能取到nums\[0]\~nums\[2]的数，也就是可能取到的是\[1, 1, 2]，也有可能取到的是\[2, 1, 1]。

以上我们可以得到结论：

* 外循环物品，内循环目标 -> 组合数
* 外循环目标，内循环物品 -> 排列数

#### [139. 单词拆分](https://leetcode-cn.com/problems/word-break/) -> 判断是否

题目说明：给定一个字符串和单词数组，问这个字符串是否可以被单词数组构成。

举例说明：s="applepenapple"，wordDict=\["apple", "pen"]，显然是可以的。返回true的结果。

```cpp
bool wordBreak(string s, vector<string>& wordDict) {
    unordered_set<string> wordSet(wordDict.begin(), wordDict.end());
    vector<bool> dp(s.size() + 1); // dp[i] 表示的是前i个字符能不能被wordDict组成
    dp[0] = true; // 0个字符的话默认为可以，作为起始条件
    for (int i = 1; i <= s.size(); i++) {
 	   for (int j = 0; j < i; j++) {
 		   string str = s.substr(j, i-j);
 		   if (dp[j] == true && wordSet.find(str) != wordSet.end()) {
 			   dp[i] = true;
 			   break;
 		   }   
 	   }
    }
    return dp[s.size()];
 }
```

#### [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/) -> 最值问题

题目说明：给定一个数值，问组成这个数的完全平方数的最少个数。其中1，4，9，16...是完全平方数

举例说明：n = 12 ，那么它可以等于 4 + 4 + 4，可以看到最少组成12的完全平方数的数量是3个。

这个题目也很好转换成背包问题，这里我们看到数值大小n可以看成背包容量，然后物品是完全平方数，每个数可以取多次也就是完全背包问题了。这样子代码就容易写出了。

```cpp
int numSquares(int n) {
    vector<int> dp(n + 1, INT_MAX);
    dp[0] = 0;
    // 注意这里是将物品放到内层循环的
    for (int i = 1; i <= n; i++) {
 	   for (int j = 1; j * j <= i; j++) {
 		   dp[i] = min(dp[i], dp[i - j*j]+1);
 	   }
    }
    return dp[n];
 }
```

## 参考文章

[代码随想录 - 背包理论基础01背包](https://www.programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html)

[代码随想录 - 背包问题理论基础完全背包](https://www.programmercarl.com/%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85.html)
