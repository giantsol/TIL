# 60. Permuation Sequence

[Link](https://leetcode.com/problems/permutation-sequence/)

```java
public String getPermutation(int n, int k) {
    StringBuilder sb = new StringBuilder();
    ArrayList<Integer> nums = new ArrayList<>();
    int fact = 1;
    for (int i = 1; i <= n; i++) {
        fact *= i;
        nums.add(i);
    }

    for (int i = 0, l = k - 1; i < n; i++) {
        fact /= (n - i);
        int index = (l / fact);
        sb.append(nums.remove(index));
        l -= index * fact;
    }

    return sb.toString();
}
```

흠.. 못풀었다!! 일단 풀이는 O(n)인데 discussion보고 베껴왔다!

설명을 보니 이해가 가는데 엄청 수학적이다.. 흠.. 어렵네..!