# 56. Merge Intervals

[Link](https://leetcode.com/problems/merge-intervals/)

```java
public int[][] merge(int[][] intervals) {
    if (intervals.length == 0) {
        return intervals;
    }

    Arrays.sort(intervals, (o1, o2) -> {
        if (o1[0] != o2[0]) {
            return o1[0] - o2[0];
        } else {
            return o1[1] - o2[1];
        }
    });

    int start = intervals[0][0];
    int end = intervals[0][1];
    final List<int[]> res = new ArrayList<>();

    for (int[] interval : intervals) {
        if (end >= interval[0]) {
            end = Math.max(end, interval[1]);
        } else {
            res.add(new int[] {start, end});
            start = interval[0];
            end = interval[1];
        }
    }
    res.add(new int[] {start, end});

    final int[][] realRes = new int[res.size()][2];
    res.toArray(realRes);
    return realRes;
}
```

소팅하는거때문에 O(nlogn)!!