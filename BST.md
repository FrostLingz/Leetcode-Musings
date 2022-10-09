## BST
[BST的增删改查](https://labuladong.github.io/algo/2/21/43/)  

BST 的特性：  
1、对于 BST 的每一个节点 node，左子树节点的值都比 node 的值要小，右子树节点的值都比 node 的值大。  
2、对于 BST 的每一个节点 node，它的左侧子树和右侧子树都是 BST。

**BST 的基础操作主要依赖「左小右大」的特性，可以在二叉树中做类似二分搜索的操作，寻找一个元素的效率很高。**

对于 BST 相关的问题，经常看到类似下面这样的代码逻辑：
```Java
void BST(TreeNode root, int target) {
    if (root.val == target)
        // 找到目标，做点什么
    if (root.val < target) 
        BST(root.right, target);
    if (root.val > target)
        BST(root.left, target);
}
```

### 判断BST的合法性
[98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)  

**常见错误:**
> 只比较当前节点和其左右孩子的大小
```java
boolean isValidBST(TreeNode root) {
    return isValidBST(root, null, null);
}

/* 限定以 root 为根的子树节点必须满足 max.val > root.val > min.val */
boolean isValidBST(TreeNode root) {
    if (root == null) return true;
    // root 的左边应该更小
    if (root.left != null && root.left.val >= root.val)
        return false;
    // root 的右边应该更大
    if (root.right != null && root.right.val <= root.val)
        return false;

    return isValidBST(root.left) && isValidBST(root.right);
}
```
出现问题的原因在于，对于每一个节点 root，代码只检查了它的左右孩子节点是否符合左小右大的原则；但是根据 BST 的定义，root 的整个左子树都要小于 root.val，整个右子树都要大于 root.val。

**正确解法1；**
> 验证二叉搜索树，就相当于判断一个序列是不是递增。
```java
class Solution {

    TreeNode pre;

    public boolean isValidBST(TreeNode root) {
        if (root == null) return true;
        
        boolean left = isValidBST(root.left);

        if (pre != null) {
            if (root.val <= pre.val) return false;
        }
        pre = root;

        boolean right = isValidBST(root.right);
        return left && right;
    }
}
```

**正确解法2；**
> 使用辅助函数，增加函数参数列表，在参数中携带额外信息，将这种约束传递给子树的所有节点。
```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, null, null);
    }

    /* 限定以 root 为根的子树节点必须满足 max.val > root.val > min.val */
    boolean isValidBST(TreeNode root, TreeNode min, TreeNode max) {
        // base case
        if (root == null) return true;
        // 若 root.val 不符合 max 和 min 的限制，说明不是合法 BST
        if (min != null && root.val <= min.val) return false;
        if (max != null && root.val >= max.val) return false;
        // 限定左子树的最大值是 root.val，右子树的最小值是 root.val
        return isValidBST(root.left, min, root)
                && isValidBST(root.right, root, max);
    }
}
```

### 在 BST 中搜索元素
[700. 二叉搜索树中的搜索](https://leetcode.cn/problems/search-in-a-binary-search-tree/)
> 普通二叉树的的搜索元素方法
```java
TreeNode searchBST(TreeNode root, int target);
    if (root == null) return null;
    if (root.val == target) return root;
    // 当前节点没找到就递归地去左右子树寻找
    TreeNode left = searchBST(root.left, target);
    TreeNode right = searchBST(root.right, target);

    return left != null ? left : right;
}
```

**穷举了所有节点**，适用于所有二叉树。但没有充分利用 BST 的特殊性，把「左小右大」的特性用上。

> 其实不需要递归地搜索两边，类似二分查找思想，根据 target 和 root.val 的大小比较，就能排除一边。

```java
TreeNode searchBST(TreeNode root, int target) {
    if (root == null) {
        return null;
    }
    // 去左子树搜索
    if (root.val > target) {
        return searchBST(root.left, target);
    }
    // 去右子树搜索
    if (root.val < target) {
        return searchBST(root.right, target);
    }
    return root;
}
```


### 在 BST 中插入元素
[701. 二叉搜索树中的插入操作](https://leetcode.cn/problems/insert-into-a-binary-search-tree/)
> 虽然可能存在多种有效的插入方式，但是实际上一定可以在**叶子节点**处找到插入位置。
```java
TreeNode insertIntoBST(TreeNode root, int val) {
    // 找到空位置插入新节点
    if (root == null) return new TreeNode(val);

    if (root.val < val) 
        root.right = insertIntoBST(root.right, val);
    if (root.val > val) 
        root.left = insertIntoBST(root.left, val);
    return root;
}
```


### 在 BST 中删除元素
[450. 删除二叉搜索树中的节点](https://leetcode.cn/problems/delete-node-in-a-bst/)

> 情况 1：**A 恰好是叶子节点**，两个子节点都为空，那么直接删除即可。  
> 情况 2：**A 只有一个非空子节点**，那么它要让这个孩子接替自己的位置。  
> 情况 3：**A 有两个子节点**，为了不破坏 BST 的性质，A 必须找到左子树中最大的那个节点，或者右子树中最小的那个节点来接替自己。  
> 因为右子树中最小的节点一定是叶子节点操作比左子树最大节点方便，我们选择右子树最小节点。

```java
TreeNode deleteNode(TreeNode root, int key) {
    if (root == null) return null;
    if (root.val == key) {
        // 这两个 if 把情况 1 和 2 都正确处理了
        if (root.left == null) return root.right;
        if (root.right == null) return root.left;
        // 处理情况 3
        // 获得右子树最小的节点
        TreeNode minNode = getMin(root.right);
        // 删除右子树最小的节点
        root.right = deleteNode(root.right, minNode.val);
        // 用右子树最小的节点替换 root 节点
        minNode.left = root.left;
        minNode.right = root.right;
        root = minNode;
    } else if (root.val > key) {
        root.left = deleteNode(root.left, key);
    } else if (root.val < key) {
        root.right = deleteNode(root.right, key);
    }
    return root;
}

TreeNode getMin(TreeNode node) {
    // BST 最左边的就是最小的
    while (node.left != null) node = node.left;
    return node;
}
```
