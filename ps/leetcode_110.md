# 110. Balanced Binary Tree

[Link](https://leetcode.com/problems/balanced-binary-tree/)

```java
public boolean isBalanced(TreeNode root) {
    return loop(root) != -1;
}

private int loop(TreeNode root) {
    if (root == null) {
        return 0;
    }

    final int leftTree = loop(root.left);
    if (leftTree == -1) {
        return -1;
    }
    final int rightTree = loop(root.right);
    if (rightTree == -1) {
        return -1;
    }

    if (Math.abs(leftTree - rightTree) > 1) {
        return -1;
    }

    return Math.max(leftTree, rightTree) + 1;
}
```

훔.. discussion보고 풀었다!! 아 recursion은 너무 헷갈린다!!!