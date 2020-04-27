# 93. Restore IP Addresses

[Link](https://leetcode.com/problems/restore-ip-addresses/)

```java
public List<String> restoreIpAddresses(String s) {
    final List<Integer> acc = new ArrayList<>(4);
    final List<String> res = new ArrayList<>();
    loop(s, 0, acc, res);
    return res;
}

private void loop(String s, int start, List<Integer> acc, List<String> res) {
    if (start == s.length()) {
        return;
    } else if (acc.size() == 3) {
        if (s.charAt(start) == '0') {
            if (start == s.length() - 1) {
                acc.add(0);
                res.add(createIPString(acc));
                acc.remove(acc.size() - 1);
            }
            return;
        }
        try {
            int num = Integer.parseInt(s.substring(start));
            if (num >= 0 && num <= 255) {
                acc.add(num);
                res.add(createIPString(acc));
                acc.remove(acc.size() - 1);
            }
        } catch (NumberFormatException ignored) { }
    } else {
        if (s.charAt(start) == '0') {
            acc.add(0);
            loop(s, start + 1, acc, res);
            acc.remove(acc.size() - 1);
        } else {
            for (int i = start; i < s.length(); i++) {
                int num = Integer.parseInt(s.substring(start, i + 1));
                if (num >= 0 && num <= 255) {
                    acc.add(num);
                    loop(s, i + 1, acc, res);
                    acc.remove(acc.size() - 1);
                } else {
                    break;
                }
            }
        }
    }
}

private String createIPString(List<Integer> acc) {
    final StringBuilder sb = new StringBuilder();
    for (int i : acc) {
        sb.append(i);
        sb.append('.');
    }
    sb.deleteCharAt(sb.length() - 1);
    return sb.toString();
}
```

아 내가 너무 대충 생각했다! 그렇게 어려운 문제는 아니었는데 예시 샘플들을 충분히 생각해두지 않아서
6번 시도만에 풀렸다!!

반면교사삼아야지!!