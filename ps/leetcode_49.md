# 49. Group Anagrams

[Link](https://leetcode.com/problems/group-anagrams/)

```java
public List<List<String>> groupAnagrams(String[] strs) {
    final Map<String, List<String>> map = new HashMap<>();
    for (String word : strs) {
        final char[] charArr = word.toCharArray();
        Arrays.sort(charArr);
        final String sortedWord = String.valueOf(charArr);
        if (!map.containsKey(sortedWord)) {
            map.put(sortedWord, new ArrayList<>());
        }
        map.get(sortedWord).add(word);
    }

    final List<List<String>> result = new ArrayList<>();
    for (String key : map.keySet()) {
        result.add(map.get(key));
    }

    return result;
}
```

톼임 컴플뤡시티: O(n * klogk) (n: len of strs, k: max len of str in strs)

스풰이스 컴플뤡시티: O(n)