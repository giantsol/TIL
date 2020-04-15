# 70. Climbing Stairs

[Link](https://leetcode.com/problems/climbing-stairs/)

```java
public int climbStairs(int n) {
    final int[] memo = new int[n + 1];
    for (int i = 0; i <= n; i++) {
        if (i <= 2) {
            memo[i] = i;
        } else {
            memo[i] = memo[i - 1] + memo[i - 2];
        }
    }

    return memo[n];
}
```

time: O(n), space: O(n)!
간단한 dynamic programming 문제!