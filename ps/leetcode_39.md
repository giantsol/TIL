# 39. Combination Sum

[Link](https://leetcode.com/problems/combination-sum/)

```java
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    Arrays.sort(candidates);
    final List<List<Integer>> result = new ArrayList<>();
    loop(candidates, 0, target, new ArrayList(), result);
    return result;
}

private void loop(int[] candidates, int index, int target, List<Integer> acc, List<List<Integer>> res) {
    if (target < 0) {
        return;
    } else if (target == 0) {
        res.add(new ArrayList<>(acc));
    } else {
        for (int i = index; i < candidates.length; i++) {
            final int cand = candidates[i];
            acc.add(cand);
            loop(candidates, i, target - cand, acc, res);
            acc.remove(acc.size() - 1);
        }
    }
}
```

전형적인 backtracking 문제!

처음엔 heap이나 queue써서 iteratively하게 풀어보려고했는데 못했다!! 그리고 지금도 별 관심은 없음..
이 경우 recursive로해도 stackoverflow가 일어날 일은 없어서!

Time complexity는 O(2^n), space는 대략 O(n)!?
