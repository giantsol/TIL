# 116. Populating Next Right Pointers in Each Node

```java
public Node connect(Node root) {
    final Node[] arr = new Node[12];
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

recursive하게 풀 수 있는 재미있는 문제!!
타임 O(n) 스페이스 O(logn)이닷!