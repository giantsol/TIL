# 18. 4Sum

[Link](https://leetcode.com/problems/4sum/)

```kotlin
fun fourSum(nums: IntArray, target: Int): List<List<Int>> {
    if (nums.size <= 3) {
        return emptyList()
    }

    nums.sort()

    val result = hashSetOf<Quad>()
    for (i in 0 until nums.size - 3) {
        for (j in (i + 1) until nums.size - 2) {
            val first = nums[i]
            val second = nums[j]

            var start = j + 1
            var end = nums.size - 1
            while (end > start) {
                val sum = first + second + nums[start] + nums[end]
                if (sum == target) {
                    result.add(Quad(first, second, nums[start], nums[end]))
                    start += 1
                } else if (sum < target) {
                    start += 1
                } else {
                    end -= 1
                }
            }
        }
    }

    return result.toList().map { it.toList() }
}

data class Quad(val first: Int,
                val second: Int,
                val third: Int,
                val fourth: Int) {
    fun toList(): List<Int> = listOf(first, second, third, fourth)
}
```

Time complexity: O(n^3)

TwoSum, ThreeSum이랑 똑같은 원리.
O(n^3)이 최적의 솔루션!
밥먹자마자 자버려가지고 속이 안좋다 윽..