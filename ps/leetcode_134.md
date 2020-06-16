# 134. Gas Station

[Link](https://leetcode.com/problems/gas-station/)

```java
public int canCompleteCircuit(int[] gas, int[] cost) {
    for (int i = 0; i < gas.length; i++) {
        if (canComplete(gas, cost, i)) {
            return i;
        }
    }
    return -1;
}

private boolean canComplete(int[] gas, int[] cost, int start) {
    int remaining = 0;
    for (int i = 0; i < gas.length; i++) {
        final int index = (start + i) % gas.length;
        remaining += gas[index] - cost[index];
        if (remaining < 0) {
            return false;
        }
    }

    return true;
}
```

깊이 생각 안하고 구냥 O(n^2) 타임. Discussion보니까 O(n)으로 최적화 가능하더라!!

슬슬 혼자 알고리즘푸는거 지루해지기 시작했네 ㅋㅋㅋㅋㅋㅋㅋ