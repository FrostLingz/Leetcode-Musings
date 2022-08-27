
void BST(TreeNode root, int target) {
    if (root.val == target)
        // Find the target Do something
    if (root.val < target) 
        BST(root.right, target);
    if (root.val > target)
        BST(root.left, target);
}
