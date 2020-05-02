# 100. Same Tree

[Link](https://leetcode.com/problems/same-tree/)

```java
public boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null || q == null) {
        return p == null && q == null;
    }

    return p.val == q.val && isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```

recursion과 함께하는 쉬운 문제!