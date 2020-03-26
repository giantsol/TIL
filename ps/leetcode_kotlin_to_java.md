# Kotlin to Java

아니 요즘 시대에 코틀린을 아직도 지원하지 않는 곳이 있딴말야?
여태 코틀린으로만 문제풀었는데...

이제부터 알고리즘은 자바로 풀어야겠다!!
그런 의미에서 코틀린으로 풀었던거 몇개 자바로 전환해보는 연습을 해보자!

## 616. Add Bold Tag in String

[Link](https://leetcode.com/problemset/all/?search=add%20bold%20)

```java
public String addBoldTag(String s, String[] dict) {
    final ArrayList<Pair<Integer, Integer>> intervals = new ArrayList<>();
    for (String sub : dict) {
        int start = s.indexOf(sub);
        while (start != -1) {
            intervals.add(new Pair<>(start, start + sub.length()));
            start = s.indexOf(sub, start + 1);
        }
    }

    intervals.sort((o1, o2) -> {
        int diff = o1.getFirst() - o2.getFirst();
        if (diff != 0) {
            return diff;
        } else {
            return o1.getSecond() - o2.getSecond();
        }
    });

    if (intervals.isEmpty()) {
        return s;
    }

    final ArrayList<Pair<Integer, Integer>> mergedIntervals = new ArrayList<>();
    int start = intervals.get(0).getFirst();
    int end = intervals.get(0).getSecond();
    for (int i = 1; i < intervals.size(); i++) {
        final int nextStart = intervals.get(i).getFirst();
        final int nextEnd = intervals.get(i).getSecond();
        if (nextStart > end) {
            mergedIntervals.add(new Pair<>(start, end));
            start = nextStart;
            end = nextEnd;
        } else {
            end = Math.max(end, nextEnd);
        }
    }
    mergedIntervals.add(new Pair<>(start, end));

    final ArrayList<String> result = new ArrayList<>();
    for (char c : s.toCharArray()) {
        result.add(String.valueOf(c));
    }
    for (int i = mergedIntervals.size() - 1; i >= 0; i--) {
        result.add(mergedIntervals.get(i).getSecond(), "</b>");
        result.add(mergedIntervals.get(i).getFirst(), "<b>");
    }

    return String.join("", result);
}
```

어우 올만에 자바로 짤라니까 자꾸 val var쓰려고해서 어색하네 ㅋㅋㅋㅋㅋㅋㅋ