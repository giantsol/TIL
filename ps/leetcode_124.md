# 124. Binary Tree Maximum Path Sum

[Link](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

```java
int result = Integer.MIN_VALUE;

public int maxPathSum(TreeNode root) {
    loop(root);
    return result;
}

private int loop(TreeNode root) {
    int maxVal = root.val;
    result = Math.max(result, maxVal);

    Integer leftVal = null;
    if (root.left != null) {
        leftVal = loop(root.left);
        result = Math.max(result, leftVal);
        result = Math.max(result, leftVal + root.val);
        maxVal = Math.max(maxVal, leftVal + root.val);
    }

    if (root.right != null) {
        final int rightVal = loop(root.right);
        result = Math.max(result, rightVal);
        result = Math.max(result, rightVal + root.val);
        maxVal = Math.max(maxVal, rightVal + root.val);
        if (leftVal != null) {
            result = Math.max(result, leftVal + rightVal + root.val);
        }
    }

    return maxVal;
}
```

오 다섯번만에 풀었다! 엣지케이스들이 많았다.
discussion보니까 global variable을 쓰는건 마찬가진데 더 짧게 쓸 수 있는 방법은 있네!

타임 O(n), 스페이스 O(logN)!