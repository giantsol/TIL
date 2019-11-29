# Preparing coding interview

Algorithm problems share some simple patterns:

## Pattern 1: If the given input is sorted (array, list, or matrix), then we will use a variation of **Binary Search** or a **Two Pointers** strategy.

### Sample 1: Binary Search

#### Problem Statement
 
Find the maximum value in a given Bitonic array. 
An array is considered bitonic if it is monotonically increasing and then monotonically decreasing.
Monotonically increasing or decreasing means that for any index i in the array, arr[i] != arr[i+1].

```
Example: Input: [1, 3, 8, 12, 4, 2], Output: 12
```

#### Solution

A bitonic array is a sorted array; the only difference is that its first part is sorted in ascending order, and the second part is sorted in descending order.
We can use a variation of Binary Search to solve this problem.
Remember that in Binary Search we have start, end, and middle indices and in each step we reduce our search space by moving start or end.
Since no two consecutive numbers are the same (as the array is monotonically increasing or decreasing), whenever we calculate the middle index for Binary Search, we can compare the numbers pointed out by the index middle and middle+1 to find if we are in the ascending or the descending part.
So:

1. If arr[middle] > arr[middle + 1], we are in the second (descending) part of the bitonic array. Therefore, our required number could either be pointed out by middle or will be before middle. This means we will do end = middle.
2. If arr[middle] <= arr[middle + 1], we are in the first (ascending) part of the bitonic array. Therefore, the required number will be after middle. This means we do start = middle + 1.

We can break when start == end. Due to the above two points, both start and end will point at the maximum number of the Bitonic array.

```java
class MaxInBitonicArray {
  public static int findMax(int[] arr) {
    int start = 0, end = arr.length - 1;
    while (start < end) {
      int mid = start + (end - start) / 2;
      if (arr[mid] > arr[mid + 1]) {
        end = mid;
      } else {
        start = mid + 1;
      }
    }

    // at the end of the while loop, 'start == end'
    return arr[start];
  }
}
```

### Sample 2: Two Pointers

#### Problem Statement

Given an array of sorted numbers and a target sum, find a pair in the array whose sum is equal to the given target.
Write a function to return the indices of the two numbers (i.e., the pair) such that they add up to the given target.

```
Example: Input: [1, 2, 3, 4, 6], target = 6, Output: [1, 3] (The numbers at index 1 and 3 add up to 6: 2+4=6)
```

#### Solution

Since the given array is sorted, a brute-force solution could be to iterate through the array, taking one number at a time and searching for the second number through Binary Search.
The time complexity of this algorithm will be O(N*logN). Can we do better than this?

We can follow the Two Pointers approach.
We will start with one pointer pointing to the beginning of the array and another pointing at the end.
At every step, we will see if the numbers pointed by the two pointers add up to the target sum.
If they do, we’ve found our pair. Otherwise, we’ll do one of two things:

1. If the sum of the two numbers pointed by the two pointers is greater than the target sum, we need a pair with a smaller sum. So, to try more pairs, we can decrement the end-pointer.
2. If the sum of the two numbers pointed by the two pointers is smaller than the target sum, this means that we need a pair with a larger sum. So, to try more pairs, we can increment the start-pointer.

```java
class PairWithTargetSum {
  public static int[] search(int[] arr, int targetSum) {
    int left = 0, right = arr.length - 1;
    while (left < right) {
      // comparing the sum of two numbers to the 'targetSum' can cause integer overflow
      // so, we will try to find a target difference instead
      int targetDiff = targetSum - arr[left];
      if (targetDiff == arr[right])
        return new int[] { left, right }; // found the pair

      if (targetDiff > arr[right])
        left++; // we need a pair with a bigger sum
      else
        right--; // we need a pair with a smaller sum
    }
    return new int[] { -1, -1 };
  }
}
```

## Pattern 2: If we’re dealing with top/maximum/minimum/closest k elements among n elements, we will use a **Heap**.

### Sample

#### Problem Statement

Given an array of points in a 2D plane, find K closest points to the origin.

```
Example: Input: points = [[1,2],[1,3]], K = 1, Output: [[1,2]]
```

#### Solution

We can use a Max Heap to find K points closest to the origin.
We can start by pushing K points in the heap.
While iterating through the remaining points, if a point (say P) is closer to the origin than the top point of the max-heap, we will remove that top point from the heap and add P to always keep the closest points in the heap.

```java
class Point {
  int x;
  int y;

  public Point(int x, int y) {
    this.x = x;
    this.y = y;
  }

  public int distFromOrigin() {
    // ignoring sqrt
    return (x * x) + (y * y);
  }
}

class KClosestPointsToOrigin {
  public static List<Point> findClosestPoints(Point[] points, int k) {
    PriorityQueue<Point> maxHeap = new PriorityQueue<>((p1, p2) -> p2.distFromOrigin() - p1.distFromOrigin());
    // put first 'k' points in the max heap
    for (int i = 0; i < k; i++)
      maxHeap.add(points[i]);

    // go through the remaining points of the input array, if a point is closer to 
    // the origin than the top point of the max-heap, remove the top point from 
    // heap and add the point from the input array
    for (int i = k; i < points.length; i++) {
      if (points[i].distFromOrigin() < maxHeap.peek().distFromOrigin()) {
        maxHeap.poll();
        maxHeap.add(points[i]);
      }
    }

    // the heap has 'k' points closest to the origin, return them in a list
    return new ArrayList<>(maxHeap);
  }
}
```

## Pattern 3: If we need to try all combinations (or permutations) of the input, we can either use **recursive Backtracking** or **iterative Breadth-First Search**.

### Sample

#### Problem Statement

Given a set with distinct elements, find all of its distinct subsets.

```
Example: Input: [1, 5, 3]
Output: [], [1], [5], [3], [1,5], [1,3], [5,3], [1,5,3]
```

#### Solution

To generate all subsets of the given set, we can use the Breadth-First Search (BFS) approach.
We can start with an empty set, iterate through all numbers one-by-one, and add them to existing sets to create new subsets.

Let’s take the aforementioned example to go through each step of our algorithm:

Given set: [1, 5, 3]

1. Start with an empty set: [[]]
2. Add the first number (1) to all the existing subsets to create new subsets: [[], [1]];
3. Add the second number (5) to all the existing subsets: [[], [1], [5], [1,5]];
4. Add the third number (3) to all the existing subsets: [[], [1], [5], [1,5], [3], [1,3], [5,3], [1,5,3]].

```java
class Subsets {
  public static List<List<Integer>> findSubsets(int[] nums) {
    List<List<Integer>> subsets = new ArrayList<>();
    // start by adding the empty subset
    subsets.add(new ArrayList<>());
    for (int currentNumber : nums) {
      // we will take all existing subsets and insert the current number in them to
      // create new subsets
      int n = subsets.size();
      for (int i = 0; i < n; i++) {
        // create a new subset from the existing subset and 
        // insert the current element to it
        List<Integer> set = new ArrayList<>(subsets.get(i));
        set.add(currentNumber);
        subsets.add(set);
      }
    }
    return subsets;
  }
}
```


## Pattern 4: Most of the questions related with Trees or Graphs can be solved either through **Breadth-First Search** or **Depth-First Search**.

## Pattern 5: Every recursive solution can be converted to an iterative solution using a stack.

## Pattern 6: If for a problem, there exists a brute-force solution in O(n^2) time and O(1) space, there must exist two other solutions: 1) Using a Map or a Set for O(n) time and O(n) space, 2) Using sorting for O(nlogn) time and O(1) space.

## Pattern 7: If the problem is asking for optimization (e.g. maximization or minimization), we will need to use **Dynamic Programming** to solve it.

## Pattern 8: If we need to find some common substring among a set of strings, we will be using a **HashMap** or a **Trie**.

## Pattern 9: If we need to search among a bunch of strings, **Trie** will be the best data structure.

## Pattern 10: If the problem involves a LinkedList and we can't use extra space, then use **Fast & Slow Pointer** approach.

# 출처

https://medium.com/better-programming/the-ultimate-strategy-to-preparing-for-the-coding-interview-ee9f7eb439f3
