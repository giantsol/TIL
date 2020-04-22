# 79. Word Search

[Link](https://leetcode.com/problems/word-search/)

```java
public boolean exist(char[][] board, String word) {
    for (int i = 0; i < board.length; i++) {
        for (int j = 0; j < board[0].length; j++) {
            if (check(board, word, i, j, 0)) {
                return true;
            }
        }
    }

    return false;
}

private boolean check(char[][] board, String word, int i, int j, int index) {
    if (index == word.length()) {
        return true;
    }
    if (i < 0 || i >= board.length || j < 0 || j >= board[0].length) {
        return false;
    }
    if (board[i][j] == word.charAt(index)) {
        board[i][j] = '+';
        if (check(board, word, i - 1, j, index + 1) ||
                check(board, word, i, j + 1, index + 1) ||
                check(board, word, i + 1, j, index + 1) ||
                check(board, word, i, j - 1, index + 1)) {
            return true;
        }
        board[i][j] = word.charAt(index);
        return false;
    }
    return false;
}
```

흠!! recursion으로 안풀어보려고 막 생각하다가 결국 그냥 recursion으로 풀었다.
stack을 어케 잘 쓰면 될거같은데 그게 오히려 더 복잡할듯.

m: row count, n: col count, l: word length 일 때 타임 O(m * n * l), 스페이스 O(l)