# 78. Subsets

[Link](https://leetcode.com/problems/subsets/)

```java
public List<List<Integer>> subsets(int[] nums) {
    final List<List<Integer>> result = new ArrayList<>();
    final Deque<List<Integer>> queue = new LinkedList<>();
    queue.offer(new ArrayList<>());

    while (!queue.isEmpty()) {
        final List<Integer> indexes = queue.pop();
        final List<Integer> e = new ArrayList<>(indexes.size());
        for (int index : indexes) {
            e.add(nums[index]);
        }
        result.add(e);

        final int startIndex = indexes.isEmpty() ? 0 : indexes.get(indexes.size() - 1) + 1;
        for (int i = startIndex; i < nums.length; i++) {
            final List<Integer> newElement = new ArrayList<>(indexes);
            newElement.add(i);
            queue.offer(newElement);
        }
    }

    return result;
}
```

recursive하게 하지 않고 iterative하게 Queue를 사용해봤다.

타임은 O(2^n)인데 스페이스가 그럼 어케되지.... 흠
악 헷갈려라