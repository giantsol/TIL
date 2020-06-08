# 131. Palindrome Partitioning

[Link](https://leetcode.com/problems/palindrome-partitioning/)

```java
public List<List<String>> partition(String s) {
    final ArrayList<List<String>> res = new ArrayList<>();
    final ArrayList<String> acc = new ArrayList<>();
    loop(s, 0, acc ,res);
    return res;
}

private void loop(String s, int start, List<String> acc, List<List<String>> res) {
    if (start >= s.length()) {
        res.add(new ArrayList<>(acc));
        return;
    }

    for (int i = start + 1; i <= s.length(); i++) {
        final String substring = s.substring(start, i);
        if (isPalindrome(substring)) {
            acc.add(substring);
            loop(s, i, acc, res);
            acc.remove(acc.size() - 1);
        }
    }
}

private boolean isPalindrome(String s) {
    if (s.isEmpty()) {
        return false;
    }
    int start = 0;
    int end = s.length() - 1;
    while (start <= end) {
        if (s.charAt(start) != s.charAt(end)) {
            return false;
        }
        start++;
        end--;
    }
    return true;
}
```

타임 생각하는게 까다롭네.. 타임은 O(n2^n), 스페이스는 O(n).