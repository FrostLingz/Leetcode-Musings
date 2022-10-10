## Tree

### Concepts
**深度：** 节点到根结点路径上的节点数（从上往下计数）
> 深度相关问题用**前序遍历**

**高度：** 节点到叶子节点路径上的节点数（从下往上计数）
> 高度相关问题用**后序遍历**

根节点的高度就是一棵树的最大深度

### Mindset

1、是否可以通过**遍历一遍二叉树**得到答案？如果可以，用一个 **traverse** 函数配合外部变量来实现，这叫 **「遍历」** 的思维模式。

2、是否可以定义一个**递归函数**，通过**子问题（子树）** 的答案推导出原问题的答案？如果可以，写出这个递归函数的定义，并充分利用这个函数的返回值，这叫 **「分解问题」** 的思维模式。

二叉树题目的递归解法可以分两类思路，第一类是遍历一遍二叉树得出答案，第二类是通过分解问题计算出答案，这两类思路分别对应着 **回溯算法核心框架** 和 **动态规划核心框架**。

> 无论使用哪种思维模式，都需要思考：  
> 如果单独抽出一个二叉树节点，它需要做什么事情？需要在什么时候（前/中/后序位置）做？  
> 其他的节点不用你操心，递归函数会帮你在所有节点上执行相同的操作。

### Traverse

#### PreOrder | InOrder | PostOrder

#### Level Order

#### Iterator

### Construction
[二叉树构造篇](https://labuladong.github.io/algo/2/21/38/)  

二叉树的构造问题一般都是使用「分解问题」的思路：构造整棵树 = **根节点 + 构造左子树 + 构造右子树**。  
想办法确定根节点的值，把根节点做出来，然后递归构造左右子树即可。

### LCA(Lowest Common Ancestor)
[二叉树的最近公共祖先](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_62987959e4b01a4852072fa5/1)

先看一个find方法，在一颗二叉树中寻找值为 val1 或 val2 的节点。

 ```java
 // 定义：在以 root 为根的二叉树中寻找值为 val1 或 val2 的节点
TreeNode find(TreeNode root, int val1, int val2) {
    // base case
    if (root == null) {
        return null;
    }
    // 前序位置，看看 root 是不是目标值
    if (root.val == val1 || root.val == val2) {
        return root;
    }
    // 去左右子树寻找
    TreeNode left = find(root.left, val1, val2);
    TreeNode right = find(root.right, val1, val2);
    // 后序位置，已经知道左右子树是否存在目标值

    return left != null ? left : right;
}
```
[236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)  
给你输入一棵不含重复值的二叉树，以及存在于树中的两个节点 p 和 q，请你计算 p 和 q 的最近公共祖先节点。
 > 如果一个节点能够在它的**左右子树**中分别找到 p 和 q，则该节点为 LCA 节点  
 > 第一种情况：在 find 函数的后序位置，如果发现 left 和 right 都非空，就说明当前节点是 LCA 节点   
 > 第二种情况：在 find 函数的前序位置，如果找到一个值为 val1 或 val2 的节点则直接返回   
 > 这就要用到之前实现的 find 函数了，只需在后序位置添加一个判断逻辑，即可改造成寻找最近公共祖先的解法代码
 
 ```java
 TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    return find(root, p.val, q.val);
}

// 在二叉树中寻找 val1 和 val2 的最近公共祖先节点
TreeNode find(TreeNode root, int val1, int val2) {
    if (root == null) {
        return null;
    }
    // 前序位置
    if (root.val == val1 || root.val == val2) {
        // 如果遇到目标值，直接返回
        return root;
    }
    TreeNode left = find(root.left, val1, val2);
    TreeNode right = find(root.right, val1, val2);
    // 后序位置，已经知道左右子树是否存在目标值
    if (left != null && right != null) {
        // 当前节点是 LCA 节点
        return root;
    }

    return left != null ? left : right;
}
 ```
