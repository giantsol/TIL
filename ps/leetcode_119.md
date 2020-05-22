# 119. Pascal's Triangle II

[Link](https://leetcode.com/problems/pascals-triangle-ii/)

```java
public List<Integer> getRow(int rowIndex) {
    final int[] prev = new int[rowIndex + 1];
    final int[] cur = new int[rowIndex + 1];
    int counter = 0;
    while (counter <= rowIndex) {
        for (int i = 0; i < counter; i++) {
            prev[i] = cur[i];
        }

        cur[0] = 1;
        for (int i = 1; i <= counter; i++) {
            cur[i] = prev[i - 1] + prev[i];
        }

        counter++;
    }

    final List<Integer> res = new ArrayList<>(rowIndex + 1);
    for (int i : cur) {
        res.add(i);
    }
    return res;
}
```

스페이스 O(k), 타임 O(k^2)!! 더 간략하게 코드를 쓰는 방법은 discussion에 나와있지만 complexity는 동일!