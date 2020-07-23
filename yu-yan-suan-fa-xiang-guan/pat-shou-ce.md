# PAT手册

## 考纲

官方的介绍网站：[https://www.patest.cn/introduction](https://www.patest.cn/introduction)

### 甲级的考察内容

> #### 乙级要求：
>
> 1. 基本的C/C++的代码设计能力，以及相关开发环境的基本调试技巧；
>
> 2. 理解并掌握最基本的数据存储结构，即：数组、链表；
>
> 3. 理解并熟练编程实现与基本数据结构相关的基础算法，包括递归、排序、查找等；
>
> 4. 能够分析算法的时间复杂度、空间复杂度和算法稳定性；
>
> 5. 具备问题抽象和建模的初步能力，并能够用所学方法解决实际问题。
>
> #### 在达到乙级要求的基础上，还要求：
>
> 1. 具有充分的英文阅读理解能力；
>
> 2. 理解并掌握基础数据结构，包括：**线性表**、**树**、**图**；
>
> 3. 理解并熟练编程实现经典高级算法，包括**哈希映射**、**并查集**、**最短路径**、**拓扑排序**、**关键路径**、**贪心**、**深度优先搜索**、**广度优先搜索**、**回溯剪枝**等；
>
> 4. 具备较强的问题抽象和建模能力，能实现对复杂实际问题的模拟求解。

### 近几年的考察题型汇总

PAT甲级考试总共有4道题，考察的类型分别如下\(参考自浒鱼鱼的[文章](https://blog.csdn.net/allisonshing/article/details/104213959)\)：

| 题号 | 考察知识点 |  |
| :--- | :--- | :--- |
| 1 | 字符串、素数、数学问题 |  |
| 2 | 链表、Hash函数、排序 |  |
| 3 | 图、并查集、\(平衡、搜索\)二叉树 |  |
| 4 | LCA、树、拓扑排序、Dijkstra、堆 |  |



## 链表

甲级题目中大部分考察的静态链表，也就是使用数组的index作为指针。

考察题目：

| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1 | 找到单词链表的共同后缀 | [1032 Sharing \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805460652113920) |
| 2 | 对链表进行排序 | [1052 Linked List Sorting \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805425780670464) |

第2道题参考了柳神的做法，用了一个取巧的办法，实际并没有对链表进行实质意义上的排序，而是记录了每个节点的自身的地址，实际上并没有对节点的地址进行调整。



## 树

对于二叉树的遍历方式有三种分别是：

前序\(先序\)：根左右（根节点，左孩子，右孩子）

中序：左根右（左孩子，根节点，右孩子），如果是二叉搜索树，这个访问结果序列为有序序列

后序：左右根（左孩子，右孩子，根节点）

不加证明地给出，已知前序中序，或者中序后序可以唯一确定一颗二叉树，给定前序后序则不一定可以唯一确定一颗二叉树。

```c
struct node {
    int val;
    node *left, *right;
};
// 三个遍历方式的递归实现是很容易记的，按照访问顺序依次写即可
void preOrder(node *root) {
    if (root == NULL) return;
    printf("%d", root->val);
    preOrder(root->left);
    preOrder(root->right);
}

void inOrder(node *root) {
    if (root == NULL) return;
    inOrder(root->left);
    printf("%d", root->val);
    inOrder(root->right);
}

void postOrder(node *root) {
    if (root == NULL) return;
    postOrder(root->left);
    postOrder(root->right);
    printf("%d", root->val);
}

// 层序遍历
void layerOrder(node *root){
    if (root == NULL) return;
    queue<node*> q;
    q.push(root);
    while(!q.empty()) {
        node *u = q.front();
        q.pop();
        printf("%d", u->val);
        if (root->left != NULL)
            q.push(root->left);
        if (root->right != NULL)
            q.push(root->right);
    }
}

// 怎么求一个树节点的高度(层)？
// 需要增加一个变量layer来记录结点所在的层次
struct node {
    int data;
    int layer;
    node *left;
    node *right;
};

void layerOrder(node *root) {
    if (root == NULL) return;
    queue<node*> q;
    root->layer = 1;
    q.push(root);
    while(!q.empty()) {
        node *u = q.front();
        q.pop();
        if (u->left != NULL) {
            u->left->layer = u->layer + 1;
            q.push(u->left);
        }
        if (u->right != NULL) {
            u->right->layer = u->layer + 1;
            q.push(u->right);
        }
    }
}
```

### 利用遍历序列重构二叉树

考察题目：

| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1 | 依据后序中序输出前序 | [1020.Tree Traversals \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805485033603072) |
| 2 | 给定前序后序输出中序\(不一定唯一\) | [1119.Pre- and Post-order Traversals \(30分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805353470869504) |
| 3 | 给定前序中序，输出后序 | [1086.Tree Traversals Again \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805380754817024) |

第3道题比较“含蓄”地给出前序和中序，前序是其PUSH的序列，而中序是POP的序列。

代码实例：

```c
/**
 * 依据前序中序构建二叉树
 * 前序区间[preL, preR]，中序区间[inL, inR]
 * 前序访问顺序：根左右，中序访问顺序：左根右 
 */ 
node *create(int preL, int preR, int inL, int inR) {
    if (preL > preR) return NULL;
    node *root = new node;
    root->data = pre[preL]; // 前序根左右的根节点
    int k;
    for (k = inL; k <= inR; ++k) { // 找到中序左根右的根位置
        if (in[k] == pre[preL]) 
            break;
    }
    int numLeft = k - inL; 
    // 依据中序根节点的位置分割序列构建左右子树
    root->left = create(preL + 1, preL + numLeft, inL, k - 1);
    root->right = create(preL + numLeft + 1, preR, k + 1, inR);
    return root;
}

/* 依葫芦画瓢，可以写出给定中序后序构建二叉树的过程 */
node *create(int inL, int inR, int postL, int postR){
    if (postL > postR) return NULL;
    node *root = new node;
    root->val = post[postR];
    int k;
    for (k = inL; k <= inR; ++k) {
        if (in[k] == post[postR])
            break;
    }
    int numLeft = k - inL;
    root->left = create(inL, k - 1, postL, postL + numLeft - 1);
    root->right = create(k + 1, inR, postL + numLeft, postR - 1);
    return root;
}
```

考察题目：

| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1 | 给定前序中序，输出后序 | [1138.Postorder Traversal \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805345078067200) |

这道题的要求是输出后序访问中的第一个元素，参考柳神的代码中，我们可以得到不需要构建二叉树，直接输出这颗二叉树的后序思路，具体如下：

```c
void postOrder(int preL, int inL, int inR) {
    if (inL > inR) return;
    int i = inL;
    while(in[i] != pre[preL]) i++;
    postOrder(preL + 1, inL, i - 1); // 相当于遍历左子树
    postOrder(preL + i - inL + 1, i + 1, inR); // 相当于遍历右子树
    printf("%d ", in[i]);
}

// 同样的，可以得到给定中序后序输出前序
void preOrder(int postR, int inL, int inR) {
    if (inL > inR || postR < 0) return;
    printf("%d ", post[postR]);
    int i = inL;
    while(in[i] != post[postR]) i++;
    preOrder(postR - inR + i - 1, inL, i - 1); // inR - i是右子树的结点数量
    preOrder(postR - 1, i + 1, inR);
}

```

### 平衡二叉树

考察题目：

| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1 | 求解平衡二叉树的根节点 | [1066.Root of AVL Tree \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805404939173888) |
| 2 | 创建平衡二叉树+判断完全二叉树 | [1123.Is It a Complete AVL Tree \(30分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805351302414336) |
| 3 | 创建一颗二叉搜索树 | [1099.Build A Binary Search Tree \(30分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805367987355648) |

这道题的本质就是构建一颗平衡二叉树，然后输出它的根节点即可，具体的代码解释，请查看[这里](https://linlh.gitbook.io/cs-notes/yu-yan-suan-fa-xiang-guan/ping-heng-er-cha-shu-avl)。

```c
struct node {
    int val;
    struct node *left, *right;
};

node *rotateLeft(node *root) {
    node *t = root->right;
    root->right = t->left;
    t->left = root;
    return t;
}

node *rotateRight(node *root) {
    node *t = root->left;
    root->left = t->right;
    t->right = root;
    return t;
}

node *rotateLeftRight(node *root) {
    root->left = rotateLeft(root->left);
    return rotateRight(root);
}

node *rotateRightLeft(node *root) {
    root->right = rotateRight(root->right);
    return rotateLeft(root);
}

int getHeight(node *root) {
    if(root == NULL) return 0;
    return max(getHeight(root->left), getHeight(root->right)) + 1;
}

node *insert(node *root, int val) {
    if (root == NULL) {
        root = new node();
        root->val = val;
        root->left = root->right = NULL;
    }
    else if (val < root->val){
        root->left = insert(root->left, val);
        if (getHeight(root->left) - getHeight(root->right) == 2)
            root = val < root->left->val ? rotateRight(root):
            rotateLeftRight(root);
    }
    else {
        root->right = insert(root->right, val);
        if (getHeight(root->left) - getHeight(root->right) == -2) 
            root = val > root->right->val ? rotateLeft(root):rotateRightLeft(root);
    }
    return root;
}
```

### 完全二叉树判断

```c
bool judge(int root) {
    if ( == -1) return true;
    queue<int> q;
    q.push(root);
    while (!q.empty()) {
        int tmp = q.front();
        q.pop();
        if (tmp != -1) {
            q.push(node[tmp].left);
            q.push(node[tmp].right);
        }
        else {
            while (!q.empty()) {
                tmp = q.front();
                q.pop();
                if (tmp != -1)
                    return false;
            }
        }
    }
    return true;
}

bool judge(node *root) {
    queue<node*> q;
    q.push(root);
    while(!q.empty()) {
        node *u = q.front();
        q.pop();
        if (u != NULL) {
            q.push(u->left);
            q.push(u->right);
        }
        else {
            while(!q.empty()) {
                u = q.front();
                q.pop();
                if (u != NULL)
                    return false;
            }
        }
    }
    return true;
}
```

### 并查集

考察的题目有：

| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1 | 找到每个人归属的社群 | [1107.Social Clusters \(30分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805361586847744) |
| 2 | 计算家族房产 | [1114.Family Property \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805356599820288) |
| 3 | 判断鸟儿是否属于同一棵树 | [1118.Birds in Forest \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805354108403712) |

```c
// 并查集主要就是两个操作，一个查找，一个合并
// 需要的数据结构为数组，存储自己的parent
int fa[maxn] = {0};

// find为algorithm中的函数，所以这里的Find首字母大写
int Find(int i) {
   int root = i;
   while(root != fa[root])
      root = fa[root];
   
   // 路径压缩
   int t = i;
   int p;
   while (t != root) {
      p = fa[t];
      fa[t] = root;
      t = p;
   }
   return root;
}

// union为关键字，写的时候要注意
void Union(int i, int j) {
   int ri = find(i);
   int rj = find(j);
   if (ri != rj)
      fa[ri] = rj;
}
```

### 寻找公共祖先

| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1 | 寻找最近公共祖先 | [1143 Lowest Common Ancestor \(30分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805343727501312) |
| 2 | 寻找最近公共祖先 | [1151 LCA in a Binary Tree \(30分\)](https://pintia.cn/problem-sets/994805342720868352/problems/1038430130011897856) |

首先对于“传统”思路建树，找结点路径，判断公共祖先，这个方法是肯定要会的，但是对于具体的题目，会有一些更为优化的算法。

这两道题中，第一道题采用，构建树，寻找两个节点的路径，在比较路径中的结点得到最近公共祖先会导致后面三个测试点超时，而第二道题使用上面这个思路则不会超时。

针对这两道题的解法都很巧妙，具体参考柳神的博客：[1143](https://blog.csdn.net/liuchuo/article/details/79618849)和[1151](https://blog.csdn.net/liuchuo/article/details/82560863)。

### 中缀&后缀表达式

```cpp
struct node {
    double num;
    char op;
    bool flag; // true表示操作数， false 表示操作符
};

string str; // 中缀表达式
queue<node> q; // 后缀表达式序列

void infixToPostfix() {
    node temp;
    stack<node> s; // 操作符栈
    map<char, int> op; // 记录操作符的优先级
    // 乘除法的优先级大于加减法
    op['+'] = op['-'] = 1;
    op['*'] = op['/'] = 2;
    for (int i = 0; i < str.length(); ) {
        // 如果是操作数，直接入队
        if (str[i] >= '0' && str[i] <='9') {
            temp.flag = true;
            temp.num = str[i++] - '0';
            while(i < str.length() && str[i] >= '0' && str[i] <= '9') {
                temp.num = temp.num * 10 + (str[i] - '0');
                i++;
            }
            q.push(temp);
        }
        else {
            temp.flag = false;
            // 操作符的话，需要判断栈顶的操作符优先级
            // 如果操作符的优先级小于等于栈顶优先级，则入队出栈
            while(!s.empty() && op[str[i]] <= op[s.top().op]) {
                q.push(s.top());
                s.pop();
            }
            temp.op = str[i];
            s.push(temp);
            i++;
        }
    }
    // 栈中可能还有其他操作符
    while(!s.empty()) {
        q.push(s.top());
        s.pop();
    }
}

double calPostfix() {
    double temp1, temp2;
    node cur, temp;
    stack<node> s; // 操作数栈
    while(!q.empty()) {
        cur = q.front();
        q.pop();
        if (cur.flag == true) s.push(cur); // 如果是操作数，压栈
        else {
            // 遇到操作符则出栈操作数进行运算
            temp2 = s.top().num;
            s.pop();
            temp1 = s.top().num;
            s.pop();
            temp.flag = true;
            if (cur.op == '+') temp.num = temp1 + temp2;
            else if(cur.op == '-') temp.num = temp1 - temp2;
            else if (cur.op == '*') temp.num = temp1 * temp2;
            else temp.num = temp1 / temp2;
            s.push(temp);
        }
    }
    return s.top().num;
}
```

### 二叉查找树的结点删除

```c

node *findMax(node *root){
    while(root->right != NULL) {
        root = root->right;
    }
    return root;
}

node* findMin(node *root) {
    while(root->left != NULL) {
        root = root->left;
    }
    return root;
}

void deleteNode(node *&root, int x) {
    if (root == NULL) return;
    if (root->data == x) {
        if (root->left == NULL && root->right == NULL) {
            free(root);
            root = NULL;
        }
        else if (root->left != NULL) {
            node *pre = findMax(root->left);
            root->data = pre->data;
            deleteNode(root->left, pre->data);
        }
        else {
            node *next = findMin(root->right);
            root->data = next->data;
            deleteNode(root->right, next->data);
        }
    }
    else if (root->data > x) {
        deleteNode(root->left, x);
    }
    else {
        deleteNode(root->right, x);
    }
}
```

### 哈夫曼树

```cpp
#include <iostream>
#include <queue>

using namespace std;

priority_queue<long long, vector<long long>, greater<long long> > q;

int main()
{
    int n;
    long long temp, x, y, ans = 0;
    scanf("%d", &n);
    for (int i = 0; i < n; ++i) {
        scanf("%lld", &temp);
        q.push(temp);
    }

    while(q.size() > 1) {
        x = q.top();
        q.pop();
        y = q.top();
        q.pop();
        q.push(x + y);
        ans += (x + y);
    }
    printf("%lld", ans);
}
```

## 图 

dfs+dijkstra 1087.All Roads Lead to Rome \(30分\)



### Dijkstra算法



```c
// 代码来自《算法笔记》和柳神的Github
// 调整了大写字母，在比赛或者考试中尽量选择好打的字母，大写字符输入较慢

// 邻接矩阵版本
// 这里为了注释的方便，一个变量写成一行，运用时直接写到一行即可
// n为顶点数目
int e[maxv][maxv]; // e为记录边的邻接矩阵, maxv为最大顶点数
int dis[maxv];     // 顶点的距离数组
bool visit[maxv];  // 访问数组
const int inf = 99999999;

fill(e[0], e[0] + maxv * maxv, inf);
fill(dis, dis + maxv, inf);

dis[0] = 0;
for (int i = 0; i < n; ++i) {
    int u = -1, minn = inf;
    // 这里的查找可以用堆排序来实现降低时间复杂度
    // 但是比赛过程再去实现一个堆排序不是我们的目的，直接遍历查找就好
    for (int j = 0; j < n; ++j) {
        if (visit[j] == false && dis[j] < minn) {
            u = j;
            minn = dis[j];
        }
    }
    if (u == -1) break;
    visit[u] = true;
    for (int v = 0; v < n; v++) {
        if (visit[v] == false && e[u][v] != inf && dis[v] > dis[u] + e[u][v])
            dis[v] = dis[u] + e[u][v];
    }
}

// 邻接表版本
struct Node {
    int v, dis;
};

vector<Node> Adj[maxv];
int n;
int d[maxv];
bool vis[maxv] = {false};

void Dijkstra(int s) {
    fill(d, d + maxv, inf);
    d[s] = 0;
    for (int i = 0; i < n; ++i) {
        int u = -1, minn = inf;
        for (int j = 0; j < n; ++j) {
            if (vis[j] == false && d[j] < minn) {
                u = j;
                minn = d[j];
            }
        }
        if (u == -1) return;
        vis[u] = true;
        for (int j = 0; j < Adj[u].size(); j++) {
            int v = Adj[u][j].v;
            if (vis[v] == false && d[u] + Adj[u][v].dis < d[v]) {
                d[v] = Adj[u][v].dis + d[v];
            }
        }
    }
}

```

### Floyd算法

```c
const int inf = 99999999;
const int maxv = 200;
int n, m;
int dis[maxv][maxv];

void Floyd() {
    for (int k = 0; k < n; ++k) {
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (dis[i][k] != inf && dis[k][j] != inf && dis[i][k]  + dis[k][j] < dis[i][j])
                        dis[i][j] = dis[i][k] + dis[k][j];
            }
        }
    }
}
```

### 拓扑排序

考察题型：

| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1 | 判断是否为序列是否满足拓扑排序 | [1146 Topological Order \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805343043829760) |

```c
vector<int> G[maxv];
int n, m, indegree[maxv];

bool topologicalSort() {
    int num = 0;
    queue<int> q;
    for (int i = 0; i < n; ++i) {
        if (indegree[i] == 0)
            q.push(i);
    }
    while(!q.empty()) {
        int u = q.front();
        printf("%d", u);
        q.pop();
        for (int i = 0; i < G[u].size(); ++i) {
            int v = G[u][i];
            indegree[v]--;
            if (indegree[v] == 0)
                q.push(v);
        }
        num++;
    }
    if (num == n) return true; // 拓扑排序成功
    else return false;
}

```

## 算术运算 

这两道题的核心是最大公约数的求法，使用辗转相除法 

| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1 | 有理数分数求和 | [1081.Rational Sum \(20分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805386161274880) |
| 2 | 有理数分数四则运算  | [1088.Rational Arithmetic \(20分\) ](https://pintia.cn/problem-sets/994805342720868352/problems/994805378443755520) |

### 最大公约数

```c
/*
 例子：求1071 和462的最大公约数
 使用的方法是：辗转相除法
 1071 = 2 X 462 + 147   a = 1071, b = 462
 462 = 3 X 147 + 21     a = 462 , b = 147
 147 = 7 X 21 + 0       a = 147 , b = 21
                        a = 21  , b = 0
 */
long long gcd (long long a, long long b) {
    return (b == 0)? abs(a) : gcd(b, a % b);
}
```

### 最小公倍数

```c
long long lcm(long long a, long long b) {
    long long d = gcd(a, b);
    return (a/d)*b;
}
```

### 素数判断

考察题目：

| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1 | 将数值分解成质因子相乘 | [1059 Prime Factors \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805415005503488) |

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

// 素数表, 找到从[2~maxn)中的所有素数
const int maxn = 101;
int prime[maxn]; 
int pNum = 0; // 素数表的下标
bool p[maxn] = {0}; // 记录[2, maxn)中每个数是否访问过
void Find_Prime() {
    for (int i = 2; i < maxn; ++i) {
        if (p[i] == false) {
            prime[pNum++] = i;
            // 存在i的倍数的数值一定不是素数, 2i, 3i ....
            for (int j = i +i; j < maxn; j += i) {
                p[j] = true;
            }
        }
    }
}
```

### 闰年判断

```c
bool isLeap(int year) {
    // 能被4整除但不能被100整除，或者是能被400整除的
    return (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
}
```

### 进制转换

```c
// 将sum转换成d进制
int ans[31], num = 0;
do {
    ans[num++] = sum % d;
    sum /= d;
} while(sum!= 0);

// d进制转换成十进制
int ans, product = 1;
while(x != 0) {
    y = y + (x % 10)*product;
    x = x/10;
    product = product * d;
}
```

### 分数

```c
struct Fraction {
    int up, down;
}

Fraction reduction(Fraction result) {
    if (result.down < 0) {
        result.down = -result.down;
        result.up = -reuslt.up;
    }
    if (result.up == 0) {
        result.down = 1
    }
    else {
        int d = gcd(abs(result.up, result.down));
        result.up /= g;
        result.down /= g;
    }
    return result;
}

Fraction add(Fraction f1, Fraction f2) {
    Fraction result;
    result.up = f1.up * f2.down + f1.down * f2.up;
    result.down = f1.down * f2.down;
    return reduction(result);
}

Fraction minus(Fraction f1, Fraction f2) {
    Fraction result;
    result.up = f1.up * f2.down - f1.down * f2.up;
    result.down = f1.down * f2.down;
    return reduction(result);
}

Fraction mul(Fraction f1, Fraction f2) {
    Fraction result;
    result.up = f1.up*f2.up;
    result.down = f1.down*f2.down;
    return reduction(result);
}

Fraction divide(Fraction f1, Fraction f2) {
    Fraction result;
    result.up = f1.up * f2.down;
    result.down = f1.down * f2.up;
    return reduction(result);
}

void showFraction(Fraction r) {
    r = reduction(r);
    if (r.down == 1) printf("%lld", r.up);
    else if (abs(r.up) > r.down) {
        pritnf("%d %d/%d", r.up / r.down, abs(r.up) % r.down, r.down);
    }
    else {
        pritnf("%d/%d", r.up, r.down);
    }
}
```

### 大数计算

```c
// 摘录自《算法笔记》P171
// 注意！这里的计算仅限于正数，负数的话需要改造

struct bign {
    int d[1000];
    int len;
    bign() {
        memset(d, 0, 1000);
        len = 0;
    }
};

// 逆向赋值的原因是当我们读取一个字符类型的数值比如123，那么最低位是放在字符串最高位的位置
bign change(char str[]) {
    bign a;
    a.len = strlen(str);
    for (int i = 0; i < a.len; ++i) {
        a.d[i] = str[a.len - i - 1] - '0'; // 注意这里是逆向赋值
    }
    return a;
}

// @param return: 0相等， 1大数a大，-1大数b大
int compare(bign a, bign b) {
    if (a.len > b.len) return 1;
    else if (a.len < b.len) return -1;
    else {
        for (int i = a.len - 1; i >= 0; --i) {
            if (a.d[i] > b.d[i]) return 1;
            else if (a.d[i] < b.d[i]) return -1;
        }
        return 0;
    }
}

bign add(bign a, bign b) {
    bign c;
    int carry = 0;
    // 注意！假定a的位数比较长，那么要保证从[b.len, a.len)这个区间中b.d[]中的数值为0
    //      否则会出错
    for (int i = 0; i < a.len || i < b.len; ++i) {
        int temp = a.d[i] + b.d[i] + carry;
        c.d[c.len++] = temp % 10;
        carry = temp / 10; 
    }
    if (carry != 0)
        c.d[c.len++] = carry;
    return c;
}

// 这个实现只能计算大数减去小数，也就是a>b，否则会出错
// 如果是 a < b得到的结果是负数，则需要改造这个函数
// 可以改造的思路是判断二者大小，转化成大数减去小数
// 然后需要在bign结构体中加入负数的标志位
bign sub(bign a, bign b) {
    bign c;
    for (int i = 0; i < a.len || i < b.len; ++i) {
        if (a.d[i] < b.d[i]) {
            a.d[i + 1]--;
            a.d[i] += 10;
        }
        c.d[c.len++] = a.d[i] - b.d[i];
    }
    while (c.len - 1 >= 1 && c.d[c.len - 1] == 0) {
        c.len--;
    }
    return c;
}

// 这里稍微有点差别，比如说147 × 35，其中147看成是大数，35看成是int
bign multi(bign a, int b) {
    bign c;
    int carry = 0;
    for (int i = 0; i < a.len; ++i) {
        int temp = a.d[i] * b + carry;
        c.d[c.len++] = temp % 10;
        carry = temp / 10;
    }
    while(carry != 0) {
        c.d[c.len++] = carry % 10;
        carry /= 10;
    }
    return c;
}

// 传入的初值要设置为0
bign divide(bign a, int b, int &r) {
    bign c;
    c.len = a.len;
    for (int i = a.len - 1; i >= 0; --i) {
        r = r * 10 + a.d[i];
        if (r < b) c.d[i] = 0; // 不够除
        else {
            c.d[i] = r / b;
            r = r % b;
        }
    }
    while(c.len - 1 >= 1 && c.d[c.len - 1] == 0) {
        c.len --;
    }
    return c;
}

```

### 哈希表构建-平方探测法

注意的要点是，当H\(key\)被占用的时候，下一次的探测位置的计算公式为H\(key\) + k^2，和H\(key\) - k^2的平方，如果超过了哈希表的长度，那么就需要对表长取模。那么探测的次数要几次呢，如果一直没找到什么时候退出？这里的判断标准是**k落在\[0, tsize\]**内，如果k大于ksize时，一定无法找到了。

**注意：这里查找的判断的范围是闭区间，\[0, tsize\]，插入是\[0, tsize\)**

考察题目：

| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1145 | 哈希表-平方探测 | [1145.Hashing - Average Search Time \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805343236767744) |

```c
int tsize, n, m, a;
cin >> tsize >> n >> m;
while(!isPrime(tsize)) tsize++;
vector<int> v(tsize);
for (int i = 0; i < n; ++i) {
    scanf("%d", &a);
    int flag = 0;
    for (int j = 0; j < tsize; j++) {// 这里j的取值范围只要[0, tsize)
        int pos = (a + j * j ) % tsize;
        if (v[pos] == 0) {
            v[pos] = a;
            flag = 1;
            break;
        }
    }
    if (!flag) printf("%d cannot be inserted.\n", a);
}
int ans = 0;
for (int i = 0; i < m; ++i) {
    scanf("%d", &a);
    // find a
    for (int j = 0; j <= tsize; j++) {
        ans ++;
        int pos = (a + j * j) % tsize;
        if (v[pos] == a || v[pos] == 0) break; 
    }
}
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

考察题型：

| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1 | 判断序列元素是否可以作为pivot | [1101.Quick Sort \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805366343188480) |

判断的思想是这样，一个元素能够作为pivot的条件是，序列的左半部分比pivot小，右半部分比pivot大，关键点在于，这个选为pivot的数值在一轮排序中是处于最后位置的，这个是这道题的核心。如果直接判断左半部分和右半部分满不满足条件的话，测试用例会超时，因为这样是$$ O(n^2)$$的时间复杂度。

这道题的其他解法，参考： [DedicateToA 的这篇文章](https://blog.csdn.net/DedicateToAI/article/details/102680110)。

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
const int maxn = 100;
// 归并排序归并的是相邻的两个区间，[L1,R1]和[L2, R2]
void merge(int A[], int L1, int R1, int L2, int R2) {
    int i = L1, j = L2; 、、
    int temp[maxn], index = 0;
    while(i <= R1 && j <= R2) {
        if (A[i] <= A[j])
            temp[index++] = A[i++];
        else 
            temp[index++] = B[j++];   
    }
    // 移动剩下的区间
    while(i <= R1) temp[index++] = A[i++]; 
    while(j <= R2) temp[index++] = B[j++];
    for(int i = 0; i < index; i++) {
        A[L1+i] = temp[i];
    }
}

void mergeSort(int A[], int left, int right) {
    if (left < right) {
        int mid = (left + right) /2;
        mergeSort(A, left, mid);
        mergeSort(A, mid+1, right);
        merge(A, left, mid, mid + 1, right);
    }
}

// 非递归版本
void mergeSort(int A[]) {
    // 每组step长度，前step/2和后step/2的元素合并
    for (int setp = 2; step / 2 <= n; step *= 2) {
        for (int i = 1; i <= n; i+= step) {
            int mid = i + step / 2 - 1;
            if (mid + 1 < n) {
                merget(A, i, mid, mid + 1, min(i + step - 1, n);
            }
        }
    }
}

// 用sort函数替代merge函数，A1089题用到
void mergeSort(int A[]) {
    for (int step = 2; step / 2 <= n; step *= 2) {
        for (int i = 1; i < n; i+= step)
            sort(A + i, A + min(i + step, n + 1));
        // 这里可以输出每一趟归并结果
    }  
}
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

|  |
| :--- |
|  |
|  |
|  |
|  |
|  |

### 堆排序

```c
const int maxn  = 100;
int heap[maxn], n = 10;// 10个元素

// 这一段是核心
void downAdjust(int low, int high) {
    int i = low, j = i * 2;
    while (j <= high) {
        if (j + 1 <= high && heap[j] < heap[j+1]) j = j + 1; // 找到结点i最大的孩子
        if (heap[i] >= heap[j]) break; // 已满足大顶堆
        swap(heap[i], heap[j]);
        i = j, j = i * 2;
    }
}

void createHeap() {
    for (int i = n / 2; i >= 1; --i) {
        downAdjust(i, n);
    }
}

void heapSort() {
    createHeap();
    for (int i = n; i > 1; --i) {
        swap(heap[i], heap[1]);
        downAdjust(1, i - 1);
    }
}
```





## 深度优先搜索和广度优先搜索

| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1 | 供应链所有价格 | [1079.Total Sales of Supply Chain \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805388447170560) |
| 2 | 供应链最大价格 | [1090.Highest Price in Supply Chain \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805376476626944) |
| 3 | 供应链最小价格 | [1106.Lowest Price in Supply Chain \(25分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805362341822464) |

这三道题的类型一致，都是深度优先搜索，只不过根据题目的要求不同判断条件不同，万变不离其宗，可以对比着学习。



### 背包问题

n件物品，每件物品的重量为w\[i\]，价值为c\[i\]，现在要选出若干件物品放入一个容量为V的背包中，使得选入背包的物品重量不超过容量V的前提下，让背包中共的物品价值最大。

```c
// sumW表示的是物品的重量，sumC表示的是物品的价值，index表示的是当前判断的物品编号
void DFS(int index, int sumW, int sumC) {
    if (index == n) {
        if (sumW <= V && sumC > maxValue) {
            maxValue = sumC;
        }
        return;
    }
    DFS(index + 1, sumW, sumC); // 不选第index件商品
    DFS(index + 1, sumW + w[index], sumC + c[index]); // 选第index件商品
}

// 剪枝版本
void DFS(int index, int sumW, int sumC) {
    if (index == n)
        return;
    DFS(index + 1, sumW, sumC);
    if (sumW + w[index] <= V) { // 只有满足了总重量小于背包容量的情况下才能选择该物品
        if (sumC + c[index] > ans) {
            ans = sumC + c[index];
        }
        DFS(index + 1, sumW + w[index], sumC + c[index]);
    }
}
```

总结：这个问题给了一类常见的DFS问题的解决办法，即给定一个序列，枚举这个序列的所有子序列（可以不连续）。

比如说，从N个整数中选择K个数，使得这个K个数的和为X，如果有多个方案，选择元素平方和最大的一个。

```c
int n, k, x, maxSumSqu = -1, A[maxn];
vector<int> temp, ans;
void DFS(int index, int nowK, int sum, int sumSqu) {
    if (nowK == k && sum == x) {
        if (sumSqu > maxSumSqu) {
            maxSumSqu = sumSqu;
            ans = temp;
        }
        return;
    }
    if (index == n || nowK > k || sum > x) return;
    temp.push_back(A[index]);
    DFS(index + 1, nowK + 1, sum + A[index], sumSqu + A[index] * A[index]);
    temp.pop_back();
    DFS(index + 1, nowK, sum, sumSqu);
}
```



## 有趣的算法问题

### 全排列

给定一个数组为1~N，如果输出它的全排列呢？（《算法笔记》P115）

拓展问题：如何输出给定数据的全排列呢？例如输入\['a', 'b', 'c'\]，\[11， 12， 13\]，我的思路：既然我们可以求出1~N的全排列，把这些全排列作为index下面，然后依据下标index输出输入的\['a', 'b', 'c'\]即可。

```cpp
const int maxn = 11;
int n, P[maxn], hashTable[maxn] = {false};

void generateP(int index) {
    if (index == n + 1) {
        for (int i = 0; i <= n; ++i) {
            printf("%d", P[i]);
        }
        printf("\n");
        return ;
    }
    for (int x = 1; x <= n; x++) {
        if (hashTable[x] = false) {
            P[index] = x;
            hashTable[x] = true;
            generate(index + 1);
            hashTable[x] = false;
        }
    }
}

// 测试
n = 3;
generateP(1); // 从P[1]开始生成
```

#### 拓展N皇后问题

| 序号 | 类型 | 题目 |
| :--- | :--- | :--- |
| 1 | 判断是否满足N皇后的排列序列 | [1128 N Queens Puzzle \(20分\)](https://pintia.cn/problem-sets/994805342720868352/problems/994805348915855360) |

这道题难度不大，核心就是判断N皇后的位置不在对角线以及同一行即可。

简单概述下N皇后问题，就是将N皇后放在N×N的棋盘上，要求是皇后不能处在同一排，同一列以及对角线上，目标是求出这样的排列方式有哪些。

这个问题为什么和全排列有关系呢，是这样的，每个N皇后的摆放位置可以是1N的位置，那么在棋牌上从左到右来看的话，就是每个皇后的位置都是从1~N中选择一个数值，这样满足了不是同一列，那么为了满足不是同一排，那么每个皇后选择的位置是不同的，这样得到的最终数值就是一个全排列，现在唯一不满的就是对角线了，所以只需要在全排列代码的基础上判断不处于对角线即可。

```cpp
int count = 0; // 统计有多少个N皇后的排列方案
void generateP(int index) {
    if (index == n + 1) {
        bool flag = true;
        for (int i = 1; i <= n; ++i) {
            for (int j = i + 1; j<= n; ++j) {
                if (abs(i - j) == abs[P[i] - P[j])) { // 对角线判断
                    flag = false;
                }
            }
        }
        if (flag) count++;
        return;
    }
    for (int x = 1; x <= n; x++) {
        if (hashTable[x] = false) {
            P[index] = x;
            hashTable[x] = true;
            generateP(index + 1);
            hashTable[x] = false;
        }
    }
}
```

### 日期处理

输出两个日期的差值，两个日期之间相差多少天，思路是对小的日期进行++，直到等于大的日期。

```cpp
// 这的代码摘录自《算法笔记》，有两个可以改进的
// 1. month数组可以减少为一维，减少空间暂用，对2月进行判断即可
// 2. isLeap调用次数较多，增加标志位进行判断即可 

// 这里设置为13的原因是保证下标从1~12月
int month[13][2] = {{0, 0}, {31, 31}, {28, 29}, {30, 30}, {31, 31}, {30, 30}, {31, 31}
                    , {31, 31}, {30, 30}, {31, 31},{30, 30}, {31, 31}};
bool isLeap(int year) {
    return (year % 4 == 0 && year % 100 != 0) || (year % 400) == 0);
}

// 这里假设了time1的时间小于time2
int time1, time2; // 日期格式为20200101的形式，求的是time1和time2相差的天数
int y1, y2, m1, m2, d1, d2;
y1 = time1 / 10000, m1 = time1 % 10000 / 100, di = time1 % 100;
y2 = time2 / 10000, m2 = time2 % 10000 / 100, d2 = time2 % 100;

int ans = 1; // 用于记录相差天数的结果
while(y1 < y2 || m1 < m2 || d1 < d2) {
    d1 ++; // 天数+1
    if (d1 = month[m1][isLeap(y1)] + 1) { // 超过一个月
        m1 ++;
        d1 = 1;
    }
    if (m1 == 13) { // 超过一年
        y1++;
        m1 == 1;
    }
    ans ++;
}
```

### 快速幂

```c
typedef long long LL;
LL binaryPow(LL a, LL b, LL m) {
    if (b==0) return 1;
    if (b % 2 == 1) return a * binaryPow(a, b - 1, m) % m;
    else {
        LL mul = binaryPow(a, b / 2, m);
        return mul * mul %m;
    }
}
```

### n!有多少个质因子P

```c
int cal(int n, int p) {
    int ans = 0;
    while(n) {
        ans += n / p;
        n /= p;
    }
    return ans;
}

int cal(int n, int p) {
    if (n / p) return 0;
    return n/p + cal(n / p, p);
}
```

### 组合数

利用公式$$C^m_n = C_{n-1}^m +C_{n-1}^{m-1}$$，这部分内容挺多，直接参考《算法笔记》P183

```c
long long C(long long n, long long m) {
    if ( == 0|| m == n) return  1;
    return C(n - 1, m) + C(n - 1, m - 1);
}
```

### 

## 注意！！

1. 使用scanf读入字符时要注意，前面加个空格，原因[参考这里](https://blog.csdn.net/weixin_46368810/article/details/105867661)

```c
// 输入为：1 2
// 这样才可以正确读取到内容
scanf(" %c %c", &a, &b);
```

  2. i++和++i

在代码里面，我们可以看到for循环的第三个表达式有两种写法，i++和++i，那么这两个区别是什么呢，要用哪一个呢？结论是用哪一个都可以，但是效率上有一点点差别，但是随着现代编译器和计算机性能的发展，这个差别几乎可以不考虑。那么差别在哪里呢？

 3. map的神奇之处

map可以直接访问未插入的元素，比如下面的代码：

```c
map<int, int> mp;
cout << mp[100] << endl; // 前面并没有执行mp[100] = 1这个操作，这里也可以直接访问mp[100]
```

 4. 数值的表示范围

题目中会给出数据的取值范围，我们需要判断相应的变量类型是否可以放得下，所以记住各个变量的表示范围是很有必要的。题目中一般给的是10的几次方的形式，所以大致范围需要重点关注。其他类型属于常规，可直接参考《算法笔记》P7.

| 类型 | 取值范围 | 大致范围 |
| :--- | :--- | :--- |
| int | -2147483548~+2147483647\($$-2^{31}  $$~$$  +2^{31}-1$$\) | $$-2 \times 10^{9}  $$~$$2 \times 10^{9} $$ |
| long long | $$-2^{63}  $$~$$  +2^{63}-1$$ | $$-9 \times 10^{18}  $$~$$9 \times 10^{18} $$ |

 5. 输入一行单词的小技巧

```c
// 方法1
char words[90][90];
int num = 0;
while(scanf("%s", words[num]) != EOF) {
    num++;
}

// 方法2
char str[90];
char words[90][90];
gets(str);
int len = strlen(str), r = 0; h = 0;
for (int i = 0; i < len; ++i) {
    if (str[i] != ' ')
        words[r][h++]=str[i];
    else {
        words[r][h] = '\0';
        r++;
        h = 0;
    }
}
```

6. 什么叫做剪枝？

和这个相关的有几个说法：

暴力法：枚举所有的情况，然后判断每一种情况是否合法，这种做法是非常朴素的，没有优化的算法

回溯法：如果到达递归边界的某层，由于一些事情导致已经不需要往任何一个子问题递归，就可以直接返回上一层，这种做法称之为回溯法

剪枝：在DFS中，通过题目限制条件来减少DFS计算量的方法称之为剪枝。至于为什么叫做剪枝，是因为DFS的遍历就如果如果树的先根遍历，剪枝就是去掉不满足条件的子树，所以称之为剪枝。《算法笔记》P305

7. STL中queue的注意事项

```c
// STL中queue的push操作只是制造了元素的一个副本入队，因此对入队元素的修改不会影响原来的值
// 二者之间没有关联
// 所以如果要对队列的元素进行修改的话，不要入队元素本身，而是要入队它们的编号(数组的话就是下标)
struct node {
    int data;
}a[3];
queue<node> q;
for (int i = 0; i < 3; ++i) {
    a[i].data = i;
    q.push(a[i]);
}
q.front().data = 5000;
printf("%d %d %d\n", a[0].data, a[1].data, a[2].data); // 0 1 2
a[0].data = 1000;
a[1].data = 2000;
a[2].data = 3000;
printf("%d", q.front().data); // 5000
```

8. 要点

要注意数据的取值范围！选择正确的数据类型！

注意数据中是否可能出现重复数据，使用大于小于，还是大于等于，小于等于

注意下标是从0开始还是从1开始!

使用printf和scanf进行输出输入，时间和cin和cout差别很大！尽量不适用cin和cout

## 参考材料

1. [柳婼大神的PAT github](https://github.com/liuchuo/PAT)
2. 《算法笔记》胡凡.曾磊.著
3. 《算法笔记 - 上机训练实战指南》 胡凡.曾磊.著
4.  浒鱼鱼的[CSDN博客](https://blog.csdn.net/allisonshing)
5. 
