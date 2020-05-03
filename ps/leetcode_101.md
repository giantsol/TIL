# 101. Symmetric Tree

[Link](https://leetcode.com/problems/symmetric-tree/)

```java
public boolean isSymmetric(TreeNode root) {
    if (root == null) {
        return true;
    }

    return loop(root.left, root.right);
}

private boolean loop(TreeNode left, TreeNode right) {
    if (left == null || right == null) {
        return left == right;
    }

    return left.val == right.val && loop(left.left, right.right) && loop(left.right, right.left);
}
```

recursion으로 쉽게 풀 수 있는 친구!!