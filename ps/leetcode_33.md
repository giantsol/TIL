# 33. Search in Rotated Sorted Array

[Link](https://leetcode.com/problems/search-in-rotated-sorted-array/)

```java
public int search(int[] nums, int target) {
    int start = 0;
    int end = nums.length - 1;
    while (end >= start) {
        final int mid = start + (end - start) / 2;
        if (target == nums[mid]) {
            return mid;
        } else if (target == nums[start]) {
            return start;
        } else if (target == nums[end]) {
            return end;
        } else {
            final int startNum = nums[start];
            final int midNum = nums[mid];
            if (midNum > startNum) {
                if (target > startNum && target < midNum) {
                    end = mid - 1;
                } else {
                    start = mid + 1;
                }
            } else {
                if (target > startNum || target < midNum) {
                    end = mid - 1;
                } else {
                    start = mid + 1;
                }
            }
        }
    }

    return -1;
}
```

심경의 변화로 java로 풀어보았다! 이런 간단한 로직은 쉽게 짜여지는데 더 복잡한 문제들에서는 kotlin이 제공하던 수많은 편의 함수들이 없으면 내가 지금 상황에서 잘 짤 수 있을지!!!

O(logn)이다.

와!! 근데 자바로 푸니까 runtime이 0ms가 나와버리네. 이게 자바로 알고리즘을 푸는 즐거움인가!?
