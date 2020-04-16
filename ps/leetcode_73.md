# 73. Set Matrix Zeroes

[Link](https://leetcode.com/problems/set-matrix-zeroes/)

```java
public void setZeroes(int[][] matrix) {
    if (matrix.length == 0 || matrix[0].length == 0) {
        return;
    }

    final int[] zeroRow = new int[matrix.length];
    final int[] zeroCol = new int[matrix[0].length];

    for (int i = 0; i < matrix.length; i++) {
        for (int j = 0; j < matrix[0].length; j++) {
            if (matrix[i][j] == 0) {
                zeroRow[i] = 1;
                zeroCol[j] = 1;
            }
        }
    }

    for (int i = 0; i < matrix.length; i++) {
        for (int j = 0; j < matrix[0].length; j++) {
            if (zeroRow[i] == 1 || zeroCol[j] == 1) {
                matrix[i][j] = 0;
            }
        }
    }
}
```

time: O(nm), space: O(m + n)

쉬웠다!!