# 102. Binary Tree Level Order Traversal

[Link](https://leetcode.com/problems/binary-tree-level-order-traversal/)

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    final List<List<Integer>> result = new ArrayList<>();
    List<Integer> acc = new ArrayList<>();

    if (root == null) {
        return result;
    }

    final Deque<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        List<TreeNode> allItems = new ArrayList<>(queue.size());
        while (!queue.isEmpty()) {
            allItems.add(queue.poll());
        }

        for (TreeNode item : allItems) {
            if (item.left != null) queue.offer(item.left);
            if (item.right != null) queue.offer(item.right);
            acc.add(item.val);
        }

        result.add(acc);
        acc = new ArrayList<>();
    }

    return result;
}
```

Queue를 사용해서 풀 수 있는 쉬운 문제!! O(n)!!