# 128. Longest Consecutive Sequence

[Link](https://leetcode.com/problems/longest-consecutive-sequence/)

```java
public int longestConsecutive(int[] nums) {
    final Set<Integer> set = new HashSet<>();
    for (int i : nums) {
        set.add(i);
    }

    int maxConseq = 0;
    for (int num : nums) {
        if (!set.contains(num - 1)) {
            int end = num + 1;
            while (set.contains(end)) end++;
            maxConseq = Math.max(maxConseq, end - num);
        }
    }
    return maxConseq;
}
```

와우.. 브릴리언트하네!! 내가 푼건 아니고 discussion보고 풀었는데 이야 까비다!

스페이스 O(n), 타임 O(n)!