# 63. Unique Paths II

[Link](https://leetcode.com/problems/unique-paths-ii/)

```java
public int uniquePathsWithObstacles(int[][] obstacleGrid) {
    final int[][] matrix = new int[obstacleGrid.length][obstacleGrid[0].length];
    for (int i = 0; i < matrix.length; i++) {
        for (int j = 0; j < matrix[0].length; j++) {
            if (obstacleGrid[i][j] == 1) {
                matrix[i][j] = 0;
            } else if (i == 0 && j == 0){
                matrix[i][j] = 1;
            } else {
                matrix[i][j] = -1;
            }
        }
    }

    for (int i = 0; i < matrix.length; i++) {
        for (int j = 0; j < matrix[0].length; j++) {
            if (matrix[i][j] == -1) {
                final int up = i == 0 ? 0 : matrix[i - 1][j];
                final int left = j == 0 ? 0 : matrix[i][j - 1];
                matrix[i][j] = up + left;
            }
        }
    }

    return matrix[matrix.length - 1][matrix[0].length - 1];
}
```

다이놔믹 프로그래밍. O(n^2)!