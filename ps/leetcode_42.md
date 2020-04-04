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