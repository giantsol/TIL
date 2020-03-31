# 36. Valid Sudoku

[Link](https://leetcode.com/problems/valid-sudoku/)

```java
public boolean isValidSudoku(char[][] board) {
    final boolean[] nums = new boolean[10];

    // check rows
    for (int i = 0; i < 9; i++) {
        reset(nums);
        for (int j = 0; j < 9; j++) {
            final char c = board[i][j];
            if (!isValid(nums, c)) {
                return false;
            }
        }
    }

    // check columns
    for (int i = 0; i < 9; i++) {
        reset(nums);
        for (int j = 0; j < 9; j++) {
            final char c = board[j][i];
            if (!isValid(nums, c)) {
                return false;
            }
        }
    }

    // check squares
    for (int i = 0; i < 9; i++) {
        reset(nums);
        for (int j = 0; j < 9; j++) {
            final char c = board[((i / 3) * 3) + (j / 3)][((i % 3) * 3) + (j % 3)];
            if (!isValid(nums, c)) {
                return false;
            }
        }
    }

    return true;
}

private void reset(boolean[] nums) {
    for (int i = 0; i < nums.length; i++) {
        nums[i] = false;
    }
}

private boolean isValid(boolean[] nums, char c) {
    if (Character.isDigit(c)) {
        final int index = Integer.parseInt(String.valueOf(c));
        if (nums[index]) {
            return false;
        }
        nums[index] = true;
    }

    return true;
}
```

스도쿠는 어짜피 항상 9x9니까 brute force로 해도 constant time complexity가 나온다.
그래서 그렇게 했는데 솔루션 보니까 String으로 변환하는거 사용해서 또 아주 스마트하게 풀어냈네!! 신기!!