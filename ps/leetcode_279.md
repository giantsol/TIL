# 279. Perfect Squares

[Link](https://leetcode.com/problems/perfect-squares/)

```java
public int numSquares(int n) {
    if (n <= 0) {
        return 0;
    }

    int[] cntPerfectSquares = new int[n + 1];
    cntPerfectSquares[0] = 0;
    for (int i = 1; i <= n; i++) {
        cntPerfectSquares[i] = Integer.MAX_VALUE;
    }

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j * j <= i; j++) {
            cntPerfectSquares[i] = Math.min(cntPerfectSquares[i], cntPerfectSquares[i - j * j] + 1);
        }
    }

    return cntPerfectSquares[cntPerfectSquares.length - 1];
}
```

못.. 못풀었다!! 음... 이런 dynamic programming 문제에 좀 취약하네..!!