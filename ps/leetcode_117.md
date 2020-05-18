# 117. Populating Next Right Pointers in Each Node II

[Link](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)

```java
public Node connect(Node root) {
    final Node[] arr = new Node[6000];
    return loop(root, arr, 0);
}

private Node loop(Node root, Node[] arr, int depth) {
    if (root == null) {
        return root;
    }

    loop(root.left, arr, depth + 1);
    if (arr[depth] != null) {
        arr[depth].next = root;
    }
    arr[depth] = root;
    loop(root.right, arr, depth + 1);

    return root;
}
```

스페이스 O(1), 타임 O(n)!!