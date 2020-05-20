# 118. Pascal's Triangle

[Link](https://leetcode.com/problems/pascals-triangle/)

```java
public List<List<Integer>> generate(int numRows) {
    final List<List<Integer>> res = new ArrayList<>();
    int n = 1;

    while (n <= numRows) {
        final List<Integer> newRow = new ArrayList<>(n);
        final int prevRowIndex = res.size() - 1;
        if (prevRowIndex < 0) {
            newRow.add(1);
            res.add(newRow);
            n++;
            continue;
        }
        final List<Integer> prevRow = res.get(prevRowIndex);
        for (int i = 0; i < n; i++) {
            final int left = i - 1 >= 0 ? prevRow.get(i - 1) : 0;
            final int right = i < n - 1 ? prevRow.get(i) : 0;
            newRow.add(left + right);
        }
        res.add(newRow);
        n++;
    }

    return res;
}
```

타임 스페이스 둘다 O(n)!

파스칼 삼각형 귀엽네