# 64. Minimum Path Sum

[Link](https://leetcode.com/problems/minimum-path-sum/)

```java
public int minPathSum(int[][] grid) {
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[0].length; j++) {
            if (i == 0 && j == 0) {
                continue;
            }

            final int top = i == 0 ? Integer.MAX_VALUE : grid[i-1][j];
            final int left = j == 0 ? Integer.MAX_VALUE : grid[i][j-1];
            grid[i][j] = Math.min(top, left) + grid[i][j];
        }
    }
    return grid[grid.length - 1][grid[0].length - 1];
}
```

쉬웠다!! O(mn)!