# 113. Path Sum II

[Link](https://leetcode.com/problems/path-sum-ii/)

```java
public List<List<Integer>> pathSum(TreeNode root, int sum) {
    final List<List<Integer>> res = new ArrayList<>();
    final List<Integer> acc = new ArrayList<>();
    loop(root, sum, acc, res);
    return res;
}

private void loop(TreeNode root, int target, List<Integer> acc, List<List<Integer>> res) {
    if (root == null) {
        return;
    } else if (root.left == null && root.right == null) {
        if (root.val == target) {
            acc.add(root.val);
            res.add(new ArrayList<>(acc));
            acc.remove(acc.size() - 1);
        }
    } else {
        acc.add(root.val);
        loop(root.left, target - root.val, acc ,res);
        loop(root.right, target - root.val, acc ,res);
        acc.remove(acc.size() - 1);
    }
}
```

백트뤡킹 문제! 타임 O(n), 스페이스 O(logn)!