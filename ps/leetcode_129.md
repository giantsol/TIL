# 129. Sum Root to Leaf Numbers

[Link](https://leetcode.com/problems/sum-root-to-leaf-numbers/)

```java
int result = 0;

public int sumNumbers(TreeNode root) {
    loop(root, 0);
    return result;
}

private void loop(TreeNode root, int acc) {
    if (root == null) {
        return;
    }

    final int curAcc = acc * 10 + root.val;

    if (root.left == null && root.right == null) {
        result += curAcc;
        return;
    }

    loop(root.left, curAcc);
    loop(root.right, curAcc);
}
```

타임 O(n) 스페이스 O(logN)!

처음엔 acc를 String으로 해갖고 Integer.parse 요걸로 했는데 그거보다 저렇게 하니까 훨씬 빠르네!!