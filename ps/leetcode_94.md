# 94. Binary Tree Inorder Traversal

[Link](https://leetcode.com/problems/binary-tree-inorder-traversal/)

```java
public List<Integer> inorderTraversal(TreeNode root) {
    final List<Integer> res = new ArrayList<>();
    loop(root, res);
    return res;
}

private void loop(TreeNode root, List<Integer> res) {
    if (root == null) {
        return;
    } else {
        loop(root.left, res);
        res.add(root.val);
        loop(root.right, res);
    }
}
```

recursive하게 풀면 쉽게 풀 수 있다!! 그래서 문제 출제자도 iterative하게 해보라고했는데 걍 안했따.

다른걸 공부해야해서!!! 