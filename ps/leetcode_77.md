# 77. Combinations

[Link](https://leetcode.com/problems/combinations/)

```java
public List<List<Integer>> combine(int n, int k) {
    final List<List<Integer>> result = new ArrayList<>();
    if (k > n) {
        return result;
    }

    final List<Integer> nums = new ArrayList<>(n);
    for (int i = 1; i <= n; i++) {
        nums.add(i);
    }
    final List<Integer> acc = new ArrayList<>(k);

    loop(nums, acc, 0, n - k, result, k);

    return result;
}

private void loop(List<Integer> nums, List<Integer> acc, int start, int end, List<List<Integer>> result, int k) {
    if (acc.size() == k) {
        result.add(new ArrayList<>(acc));
        return;
    }

    for (int i = start; i <= end; i++) {
        acc.add(nums.get(i));
        loop(nums, acc, i + 1, end + 1, result, k);
        acc.remove(acc.size() - 1);
    }
}
```

톼임 O(n^k), 스풰이스 O(n)!

단순한 backtracking 문제