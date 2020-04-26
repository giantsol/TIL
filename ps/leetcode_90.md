# 90. Subsets II

[Link](https://leetcode.com/problems/subsets-ii/)

```java
public List<List<Integer>> subsetsWithDup(int[] nums) {
    Arrays.sort(nums);
    final List<List<Integer>> res = new ArrayList<>();
    final Set<String> set = new HashSet<>();

    res.add(new ArrayList<>());
    for (int num : nums) {
        final int prevSize = res.size();
        for (int i = 0; i < prevSize; i++) {
            final List<Integer> newE = new ArrayList<>(res.get(i));
            newE.add(num);
            final String key = newE.toString();
            if (!set.contains(key)) {
                set.add(key);
                res.add(newE);
            }
        }
    }

    return res;
}
```

HashSet하나 써서 이미 넣었던건지 비교하는 식으로 했는데
discussion 보면 더 효율적으로 할 수 있는 방법이 있다.

어떻게 하는건지는 알듯!! 코드 짜기는 귀찮지만!