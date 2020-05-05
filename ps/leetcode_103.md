# 103. Binary Tree Zigzag Level Order Traversal

[Link](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

```java
public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
    final List<List<Integer>> result = new ArrayList<>();
    if (root == null) {
        return result;
    }

    final Deque<TreeNode> stack = new LinkedList<>();
    stack.push(root);
    boolean goingRight = true;
    List<Integer> acc = new ArrayList<>();

    while (!stack.isEmpty()) {
        final List<TreeNode> elements = new ArrayList<>(stack.size());
        while (!stack.isEmpty()) {
            elements.add(stack.pop());
        }

        for (TreeNode element : elements) {
            if (goingRight) {
                if (element.left != null) stack.push(element.left);
                if (element.right != null) stack.push(element.right);
            } else {
                if (element.right != null) stack.push(element.right);
                if (element.left != null) stack.push(element.left);
            }

            acc.add(element.val);
        }

        result.add(acc);
        goingRight = !goingRight;
        acc = new ArrayList<>();
    }

    return result;
}
```

이번엔 stack과 함께하는 쉬운 문제!!