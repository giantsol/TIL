# 114. Flatten Binary Tree to Linked List

[Link](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)

```java
public void flatten(TreeNode root) {
    loop(root, null);
}

private TreeNode loop(TreeNode root, TreeNode tail) {
    if (root == null) {
        return tail;
    }

    final TreeNode rightFlattened = loop(root.right, tail);
    final TreeNode leftFlattened = loop(root.left, rightFlattened);
    root.left = null;
    root.right = leftFlattened;
    return root;
}
```

후 생각하느라 어려웠다!!! 풀고나서 Discussion 보고나니 내가 한게 post-order traversal이군!

근데 global variable을 사용하면 이보다 훨씬 더 직관적으로 풀 수 있다!
타임, 스페이스 모두 O(n)!