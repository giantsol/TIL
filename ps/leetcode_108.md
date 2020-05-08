# 108. Convert Sorted Array to Binary Search Tree

[Link](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

```java
public TreeNode sortedArrayToBST(int[] nums) {
    return loop(nums, 0, nums.length - 1);
}

private TreeNode loop(int[] nums, int start, int end) {
    if (start == end) {
        return new TreeNode(nums[start]);
    } else if (start > end) {
        return null;
    }

    final int mid = start + (end - start) / 2;
    final TreeNode left = loop(nums, start, mid - 1);
    final TreeNode right = loop(nums, mid + 1, end);
    return new TreeNode(nums[mid], left, right);
}
```

타임 O(n), 스페이스 O(logn)!

의외로 많이 헤매었다!! recursive는 언제해도 참 헷갈린다 ㅋㅋㅋㅋㅋㅋ