# 75. Sort Colors

[Link](https://leetcode.com/problems/sort-colors/)

```java
public void sortColors(int[] nums) {
    int zeroInsertionPoint = 0;
    int twoInsertionPoint = nums.length - 1;
    while (zeroInsertionPoint < nums.length && nums[zeroInsertionPoint] == 0)
        zeroInsertionPoint++;
    while (twoInsertionPoint >= 0 && nums[twoInsertionPoint] == 2)
        twoInsertionPoint--;
    int i = zeroInsertionPoint;

    while (twoInsertionPoint >= i) {
        if ((nums[i] == 0 && i > zeroInsertionPoint)
                || (nums[i] == 2 && i < twoInsertionPoint)) {
            if (nums[i] == 0) {
                swap(nums, i, zeroInsertionPoint);
            } else {
                swap(nums, i, twoInsertionPoint);
            }

            while (zeroInsertionPoint < nums.length && nums[zeroInsertionPoint] == 0)
                zeroInsertionPoint++;
            while (twoInsertionPoint >= 0 && nums[twoInsertionPoint] == 2)
                twoInsertionPoint--;
        } else {
            i++;
        }
    }
}

private void swap(int[] nums, int i, int j) {
    final int temp = nums[i];
    nums[i] = nums[j];
    nums[j] = temp;
}
```

톼임 O(n), 스풰이스 O(1)!

사실 0, 1, 2의 갯수를 카운팅하고 그 갯수만큼 다시 채워넣으면 아주 간단하지만
그렇게 하지 말아보라고 적혀있어서 포인터를 사용해서 풀었다!