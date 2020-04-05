# 42. Trapping Rain Water

[Link](https://leetcode.com/problems/trapping-rain-water/)

```java
public int trap(int[] heights) {
    int prev = 0;
    int height = 1;
    final List<int[]> ranges = new ArrayList<>();
    int[] curRange = new int[] {-1, -1};
    int pointer = 0;

    while (pointer < heights.length) {
        int cur = heights[pointer];
        if (cur > prev) {
            curRange[1] = pointer;
            if (cur >= height) {
                ranges.add(curRange);
                curRange = new int[] {pointer, pointer};
                height = cur;
            }
        }
        prev = cur;
        pointer++;
    }
    ranges.add(curRange);
    ranges.remove(0);

    int res = 0;
    for (int[] range : ranges) {
        int start = range[0];
        int end = range[1];
        int minHeight = Math.min(heights[start], heights[end]);
        for (int i = start + 1; i < end; i++) {
            res += Math.max(0, minHeight - heights[i]);
        }
    }

    return res;
}
```

위에껀 O(n)이긴 한데 잘못된 풀이!! 될줄 알고 했는데 안됨!ㅋㅋㅋㅋㅋㅋㅋ

어떻게 하면 될지 생각은 드는데 그 풀이는 내일 해서 올려야지!

```java
public int trap(int[] heights) {
    if (heights.length == 0) {
        return 0;
    }

    int si = 0;
    int ei = 0;
    final List<int[]> ranges = new ArrayList<>();
    while (ei < heights.length - 1) {
        if (heights[ei + 1] == heights[ei]) {
            ei += 1;
        } else {
            ei += 1;
            si = ei;
        }

        if ((si - 1 >= 0 ? heights[si - 1] : -1) > heights[si] &&
                (ei + 1 < heights.length ? heights[ei + 1] : -1) > heights[ei]) {
            final int[] range = getRange(si, ei, heights);
            si = ei = range[1];
            ranges.add(range);
        }
    }

    int res = 0;
    for (int[] range : ranges) {
        final int height = Math.min(heights[range[0]], heights[range[1]]);
        for (int i = range[0] + 1; i < range[1]; i++) {
            res += height - heights[i];
        }
    }

    return res;
}

private int[] getRange(int si, int ei, int[] heights) {
    int maxSi = si;
    int maxEi = ei;
    while (si >= 0 && ei < heights.length) {
        while (ei < heights.length && heights[maxEi] <= heights[maxSi]) {
            if (heights[ei] > heights[maxEi]) {
                maxEi = ei;
                break;
            }
            ei++;
        }
        while (si >= 0 && heights[maxSi] < heights[maxEi]) {
            if (heights[si] > heights[maxSi]) {
                maxSi = si;
                break;
            }
            si--;
        }
    }

    return new int[] { maxSi, maxEi };
}
```

거의 10번의 도전 끝에 풀어냈다..!! ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ

요 풀이는 time O(n), space O(n)이다.
Discussion 보니까 one way로 time O(n), space O(1)로 푸는게 있었다!!
근데 요 풀이도 역시 처음 문제를 푼다고 가정했을 때는 생각해낼 수 없을 만한 그런 풀이었다.

이런걸 처음부터 생각해내는 사람들은 고인물이겠지?