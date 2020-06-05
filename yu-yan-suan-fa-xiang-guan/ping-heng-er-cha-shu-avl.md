# 平衡二叉树AVL

很多时候学习数据结构的时候，刚开始觉得还可以接受，到了树和图的话，就慢慢劝退了。很多时候我们觉得这个东西很难，但是实际情况呢，嗯。。它真的挺难的，但是因为它难，学习上没有及时的收获感，导致我们学而止步了。但是呢，有阻力才代表我们在上升。王强老师之前在演讲的时候说到一个例子，形式逻辑的三段论来验证他可以学好计算机，大前提是计算机是人发明的东西，小前提是人都应该懂人发明的东西，王强搞不懂计算机，结论：王强肯定不是人。虽然不是真理哈，但是也表明了我们对于学习的态度问题，既然难，说明这个内容它是包含了更多前人的智慧，老一辈花了几年几十年总结出来的内容，我们如果要求自己一两遍就弄懂，其实也不太现实（排除特别优秀的同学），所以要多给自己一点耐心，没弄懂，不是自己笨，而是自己花的时间还不够多。

AVL树的难点在于旋转问题，保证整棵树是平衡的，关于AVL树相关的概率这里不再重复，相关的数据都可以找到介绍。

旋转分为四种情况：

1. 左旋
2. 右旋
3. 先左旋后右旋
4. 先右旋后左旋

## 代码实现

### 结点

```cpp
// 定义结点，内容很清晰，结点值，左右结点指针
struct node {
    int val;
    struct node *left, *right;
};
```

### 左旋

左旋图示：

![](../.gitbook/assets/image%20%2821%29.png)

左旋代码：

```cpp
// root为上图的结点1，t为上图的结点2，t为旋转后的新的root结点
node *rotateLeft(node *root) {
    node *t = root->right; // 当前root的右孩子作为新的root，t为新的root结点
    // 由于结点2的左旋上去后，root作为它的左孩子，那么原来结点2的左孩子放到root的右孩子上
    root->right = t->left; 
    t->left = root; // 原来的root结点放到新root结点t的左孩子上
    return t;
}
```

### 右旋

右旋图示：

![](../.gitbook/assets/image%20%2818%29.png)

右旋代码：

```cpp
// root为上图的结点3，t为上图的结点2，t为旋转后的新的root结点
node *rotateRight(node *root) {
    node *t = root->left;  // 右旋是root的左孩子作为新的root
    // 由于结点2的右旋上去后，root作为它的右孩子，那么原来结点2的右孩子放到root的左孩子上
    root->left = t->right; 
    t->right = root; // 原来的root结点放到新root结点t的右孩子上
    return t;
}
```

### 先左旋后右旋

图示：

![](../.gitbook/assets/image%20%2824%29.png)

代码：

```cpp
// 代码很明确的，先将root的左孩子左旋，然后root再右旋
node *rotateLeftRight(node *root) {
    root->left = rotateLeft(root->left);
    return rotateRight(root);
}
```

### 先右旋后左旋

图示：

![](../.gitbook/assets/image%20%2825%29.png)

代码：

```cpp
// 代码很明确的，先将root的右孩子右旋，然后root再左旋
node *rotateRightLeft(node *root) {
    root->right = rotateRight(root->right);
    return rotateLeft(root);
}
```

我们可以看到，在对于旋转的代码上实现是比较明确的，结合图例很好理解，难点在于我们插入的时候根据什么来判断要进行何种旋转，下面我们来看下插入代码的实现：

### 插入代码

```cpp
node *insert(node *root, int val) {
    if (root == NULL) { // root为空则创建新结点
        root = new node();
        root->val = val;
        root->left = root->right = NULL;
    }
    else if (val < root->val){ // 插入结点的值小于root，则插入root的左子树上
        root->left = insert(root->left, val); // 递归调用，直到插入到叶子结点上
        if (getHeight(root->left) - getHeight(root->right) == 2) // 插入完成后，判断树是否失衡
            // 插入值小于root结点左孩子的值，只需要右旋，否则先左旋后右旋
            root = val < root->left->val ? rotateRight(root):rotateLeftRight(root);
    }
    else { // 插入结点的值大于等于root，则插入root的右子树上
        root->right = insert(root->right, val);
        if (getHeight(root->left) - getHeight(root->right) == -2) 
            // 插入值大于root结点右孩子的值，只需要左旋，否则先右旋后左旋
            root = val > root->right->val ? rotateLeft(root):rotateRightLeft(root);
    }
    return root;
}
```

这里的核心代码是判断什么时候进行何种旋转的代码，总结：

* 插入到左子树
  * 插入值小于最小子树root结点的左孩子的值，只需要右旋
  * 插入值大于最小子树root结点的左孩子的值，先左旋有右旋
* 插入到右子树
  * 插入值大于最小子树root结点的左孩子的值，只需要左旋
  * 插入值小于最小子树root结点的左孩子的值，先右旋后左旋

可以看到，如果插入是左子树，那么最终一定是要右旋；插入是右子树，一定最后是左旋。这就很好记了。

这里提到了最小子树，那么是什么意思呢，我们上图的图示中只画了三个结点的树，实际情况中，树的规模肯定不止三个结点，那么最小子树就是要找到插入结点后，导致不平衡的那三个结点构成的最小子树。

总结简记：

插左树，大于左，则右旋，小于左，先左后右；

插右树，小于右，则左旋，大于右，先右后左；

以上，我们则可以完美地拿下AVL树了，如果还不是很明白，那就多找几篇文章来看下吧，多花点时间琢磨琢磨，一定可以把AVL树学明白的！

## Reference

参考代码来源：[https://github.com/liuchuo/PAT/blob/master/AdvancedLevel\_C%2B%2B/1066.%20Root%20of%20AVL%20Tree%20\(25\).cpp](https://github.com/liuchuo/PAT/blob/master/AdvancedLevel_C%2B%2B/1066.%20Root%20of%20AVL%20Tree%20%2825%29.cpp)

