# 55. Jump Game

[Link](https://leetcode.com/problems/jump-game/)

```java
public boolean canJump(int[] nums) {
    if (nums.length == 0) {
        return false;
    }

    int valid = nums.length - 1;
    for (int i = nums.length - 2; i >= 0; i--) {
        if (i + nums[i] >= valid) {
            valid = i;
        }
    }

    return valid == 0;
}
```

옛날에 한번 풀어봤어서 쉽게 할 수 있었다!!

가장 먼저 떠오르는 방식으로 하면 보통 time O(n^2)에 space O(n)을 아마 하게 될텐데,
이 방식으로 하면 time O(n)에 space O(1)에 할 수 있다.

놀랍!