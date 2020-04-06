# 48. Rotate Image

[Link](https://leetcode.com/problems/rotate-image/)

```java
public void rotate(int[][] matrix) {
    int matrixLen = matrix.length;
    int level = 0;
    int size = matrixLen - (level * 2);
    final int[] tl = new int[2];
    final int[] tr = new int[2];
    final int[] br = new int[2];
    final int[] bl = new int[2];
    while (size > 1) {
        for (int i = 0; i < size - 1; i++) {
            tl[0] = level; tl[1] = i + level;
            tr[0] = tl[1]; tr[1] = matrixLen - 1 - level;
            br[0] = tr[1]; br[1] = matrixLen - 1 - i - level;
            bl[0] = br[1]; bl[1] = tl[0];

            int tmp = matrix[tl[0]][tl[1]];
            int tmp2 = matrix[tr[0]][tr[1]];
            matrix[tr[0]][tr[1]] = tmp;
            tmp = tmp2;
            tmp2 = matrix[br[0]][br[1]];
            matrix[br[0]][br[1]] = tmp;
            tmp = tmp2;
            tmp2 = matrix[bl[0]][bl[1]];
            matrix[bl[0]][bl[1]] = tmp;
            matrix[tl[0]][tl[1]] = tmp2;
        }

        level++;
        size = matrixLen - (level * 2);
    }
}
```

수학 문제!! O(n)이다!

억..ㅋㅋㅋㅋㅋ discussion보니까 손쉽게 rotate할 수 있는 공식같은게 있네.
매트릭스를 거꾸로 뒤집고 대각선끼리 swapping하면 되고 막 그런게 있구나.

어쩄든 마찬가지로 O(n)이다!
