# 60. Permuation Sequence

[Link](https://leetcode.com/problems/permutation-sequence/)

```java
public String getPermutation(int n, int k) {
    StringBuilder sb = new StringBuilder();
    final List<Integer> nums = new ArrayList<>();
    int factorial = 1;
    for (int i = 1; i <= n; i++) {
        nums.add(i);
        factorial *= i;
    }

    k--;

    for (int i = n; i >= 1; i--) {
        if (nums.size() == 1) {
            sb.append(nums.get(0));
            break;
        }
        factorial /= i;
        final int index = k / factorial;
        sb.append(nums.remove(index));
        k -= index * factorial;
    }

    return sb.toString();
}
```

흠.. 못풀었다!! 일단 풀이는 O(n)인데 discussion보고 베껴왔다!

설명을 보니 이해가 가는데 엄청 수학적이다.. 흠.. 어렵네..!

---

이해했다!!
됐으!
이해할라고 썼던 로직.

```java
/**
 1234 => n = 4, k = 3
 1234 => 1
 1243 => 2
 1324 => 3

 k--

 1 (2, 3, 4) => num of 3! (n - 1)! => 6
 2 (1, 3, 4) => num of 3! (n - 1)!
 3 (1, 2, 4)
 4 (1, 2, 3)

 [1, 2, 3, 4]
 k / (n - 1)! ==> 0
 take out 1, remaining is [2, 3, 4]

 2 (3, 4) => num of 2! (n - 2)! => 2
 3 (2, 4)
 4 (2, 3)
 k / (n - 2)! ==> 1
 take out 3, remaining is [2, 4]

 2 (4) => (n - 3)! => 1
 4 (2)
 k -= prevIndex * (n - 2)! ==> k = 0
 k / (n - 3)! ==> 0
 take out 2, remaining is [4]

 if remaining.length == 1: add that

 pattern:
 k--
 res = List()
 nums = List(1, 2, 3, ... n)
 factorial = n!

 for (i in n downTo 1):
 if (nums.length == 1): res.add(nums[0]); break;
 factorial /= i
 index = k / factorial
 res.add(nums.remove(index))
 k -= index * factorial

 */
```
