# 120. Triangle

[Link](https://leetcode.com/problems/triangle/)

```java
public int minimumTotal(List<List<Integer>> triangle) {
    if (triangle.size() == 0) {
        return 0;
    }

    final Map<Pair<Integer, Integer>, Integer> memo = new HashMap<>();
    return loop(triangle, 0, 0, memo);
}

private int loop(List<List<Integer>> triangle, int row, int col, Map<Pair<Integer, Integer>, Integer> memo) {
    final Pair<Integer, Integer> coordinates = new Pair<>(row, col);
    if (memo.containsKey(coordinates)) {
        return memo.get(coordinates);
    }

    final int curValue = triangle.get(row).get(col);
    if (row == triangle.size() - 1) {
        return curValue;
    }

    final int min = curValue + Math.min(loop(triangle, row + 1, col, memo), loop(triangle, row + 1, col + 1, memo));
    memo.put(coordinates, min);
    return min;
}
```

타임 O(n), 스페이스 O(r) (r = rowCount).

Memoization을 안쓰면 타임아웃이 난다!