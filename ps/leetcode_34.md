# 34. Find First and Last Position of Element in Sorted Array

[Link](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```java
public int[] searchRange(int[] nums, int target) {
    final int[] result = new int[] {-1, -1};
    if (nums.length == 0) {
        return result;
    }

    result[0] = findFirst(nums, 0, nums.length, target);
    result[1] = findLast(nums, 0, nums.length, target);
    return result;
}

private int findFirst(int[] nums, int start, int end, int target) {
    final int pos = Arrays.binarySearch(nums, start, end, target);
    if (pos < 0) {
        return -1;
    } else {
        if (pos > 0 && nums[pos - 1] == target) {
            return findFirst(nums, start, pos, target);
        } else {
            return pos;
        }
    }
}

private int findLast(int[] nums, int start, int end, int target) {
    final int pos = Arrays.binarySearch(nums, start, end, target);
    if (pos < 0) {
        return -1;
    } else {
        if (pos < nums.length - 1 && nums[pos + 1] == target) {
            return findLast(nums, pos + 1, end, target);
        } else {
            return pos;
        }
    }
}
```

O(logn)!!

binarySearch를 직접 구현해야하나싶어서 엄청 귀찮아질뻔했는데 자바에도 Arrays.binarySearch 유틸 함수가 있어서 다행이었다!
와 근데.. 코틀린으로하다가 자바로하니까 ㅋㅋㅋㅋ runtime이 0ms 나와서 너무 신기하다 ㅋㅋ 기분 좋네!!