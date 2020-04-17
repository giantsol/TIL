# 74. Search a 2D Matrix

[Link](https://leetcode.com/problems/search-a-2d-matrix/)

```java
public boolean searchMatrix(int[][] matrix, int target) {
    if (matrix.length == 0 || matrix[0].length == 0) {
        return false;
    }

    int start = 0;
    int end = matrix.length - 1;

    while (start <= end) {
        final int mid = start + (end - start) / 2;
        if (matrix[mid][0] == target) {
            return true;
        } else if (matrix[mid][0] > target) {
            end = mid - 1;
        } else {
            start = mid + 1;
        }
    }

    final int rowIndex = start - 1;
    if (rowIndex < 0) {
        return false;
    }

    start = 0;
    end = matrix[0].length - 1;
    while (start <= end) {
        final int mid = start + (end - start) / 2;
        if (matrix[rowIndex][mid] == target) {
            return true;
        } else if (matrix[rowIndex][mid] > target) {
            end = mid - 1;
        } else {
            start = mid + 1;
        }
    }

    return false;
}
```

O(logn)!

근데 구멍들을 좀 놓쳐서 3번째에 성공했다. 예컨대 ```if (rowIndex < 0) return false;``` 부분?
생각지못했다!!