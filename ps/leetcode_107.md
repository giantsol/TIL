# 107. Binary Tree Level Order Traversal II

[Link](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)

```java
public List<List<Integer>> levelOrderBottom(TreeNode root) {
    final Deque<List<Integer>> stack = new LinkedList<>();
    final Deque<TreeNode> queue = new LinkedList<>();
    if (root == null) {
        return new ArrayList<>(stack);
    }

    queue.offer(root);
    while (!queue.isEmpty()) {
        final List<TreeNode> items = new ArrayList<>(queue.size());
        while (!queue.isEmpty()) {
            items.add(queue.poll());
        }

        final List<Integer> acc = new ArrayList<>(queue.size());
        for (TreeNode item : items) {
            if (item.left != null) queue.offer(item.left);
            if (item.right != null) queue.offer(item.right);
            acc.add(item.val);
        }

        stack.push(acc);
    }

    return new ArrayList<>(stack);
}
```

queue와 stack을 같이 써야하는 꽤 똑띠한 문제네!! 재밌었다!

스페이스랑 타임 모두 O(n)!