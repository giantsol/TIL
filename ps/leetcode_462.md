# 462. Minimum Moves to Equal Array Elements II

[Link](https://leetcode.com/problems/minimum-moves-to-equal-array-elements-ii/)

```kotlin
fun minMoves2(nums: IntArray): Int {
    nums.sort()
    val map = nums.createCounterMap()

    val mid = if (nums.size % 2 == 0) {
        val cand1 = nums[nums.size / 2]
        val cand2 = nums[nums.size / 2 - 1]
        if (map[cand1]!! > map[cand2]!!) {
            cand1
        } else {
            cand2
        }
    } else {
        nums[nums.size / 2]
    }

    var acc = 0
    for (e in nums) {
        acc += abs(e - mid)
    }

    return acc
}

private fun IntArray.createCounterMap(): HashMap<Int, Int> {
    val map = hashMapOf<Int, Int>()
    for (e in this) {
        if (e !in map) {
            map[e] = 0
        }
        map[e] = map[e]!! + 1
    }

    return map
}
```

time complexity: O(nlogn)
space complexity: O(n)

쉬웠다!!
