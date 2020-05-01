# 98. Validate Binary Search Tree

[Link](https://leetcode.com/problems/validate-binary-search-tree/)

```java
public boolean isValidBST(TreeNode root) {
    return isValid(root, Integer.MIN_VALUE, Integer.MAX_VALUE);
}

private boolean isValid(TreeNode root, int min, int max) {
    if (root == null) {
        return true;
    }
    if (root.val == Integer.MIN_VALUE && root.left != null) {
        return false;
    }
    if (root.val == Integer.MAX_VALUE && root.right != null) {
        return false;
    }

    return root.val >= min && root.val <= max
            && isValid(root.left, min, root.val - 1)
            && isValid(root.right, root.val + 1, max);
}
```

recursive하게 쉽게 풀었.. 다고 생각했지만 엣지 케이스를 놓쳐서 한번 실패했다!!
구로치.. integer overflow를 생각하지 못했다!!