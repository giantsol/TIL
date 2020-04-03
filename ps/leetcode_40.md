# 40. Combination Sum II

[Link](https://leetcode.com/problems/combination-sum-ii/)

```java
public List<List<Integer>> combinationSum2(int[] candidates, int target) {
    final Set<List<Integer>> res = new TreeSet<>((o1, o2) -> {
        final int o1Size = o1.size();
        final int o2Size = o2.size();
        if (o1Size != o2Size) {
            return o1Size - o2Size;
        } else {
            for (int i = 0; i < o1.size(); i++) {
                if (!o1.get(i).equals(o2.get(i))) {
                    return o1.get(i) - o2.get(i);
                }
            }
            return 0;
        }
    });

    if (candidates.length == 0) {
        return new ArrayList(res);
    }

    Arrays.sort(candidates);
    loop(candidates, 0, target, new ArrayList(), res);
    return new ArrayList(res);
}

private void loop(int[] cands, int index, int target, List<Integer> acc, Collection<List<Integer>> res) {
    if (target < 0) {
        return;
    } else if (target == 0) {
        res.add(new ArrayList<>(acc));
    } else {
        for (int i = index; i < cands.length; i++) {
            final int cand = cands[i];
            acc.add(cand);
            loop(cands, i + 1, target - cand, acc, res);
            acc.remove(acc.size() - 1);
        }
    }
}
```

Time complexity: O(2^n). 어쩔수없다. permutation을 돌 수 밖에!!

근데 중복 체크를 어찌할까 생각하다가 Comparator를 넘길 수 있는 TreeSet으로 했는데, discussion보니까 무슨
특정 로직으로 중복 체크를 막을 수가 있긴 하더라. 이런 다른 자료구조를 따로 안쓰더라도!

근데 귀찮아서 안봤다!