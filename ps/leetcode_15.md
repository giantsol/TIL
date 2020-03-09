# 15. 3Sum

[Link](https://leetcode.com/problems/3sum/)

```kotlin
fun threeSum(nums: IntArray): List<List<Int>> {
    val set = HashSet<Triple<Int, Int, Int>>()
    val map = hashMapOf<Int, Int>()
    for (num in nums) {
        if (num !in map) {
            map[num] = 0
        }
        map[num] = map[num]!! + 1
    }

    for (i in 0 until nums.size - 1) {
        for (j in (i + 1) until nums.size) {
            val first = nums[i]
            val second = nums[j]
            val sum = first + second
            map[first] = map[first]!! - 1
            map[second] = map[second]!! - 1
            if (map[-sum] ?: 0 > 0) {
                val arr = intArrayOf(first, second, -sum)
                arr.sort()
                val triple = Triple(arr[0], arr[1], arr[2])
                set.add(triple)
            }
            map[first] = map[first]!! + 1
            map[second] = map[second]!! + 1
        }
    }

    val result = arrayListOf<List<Int>>()
    for (element in set) {
        result.add(listOf(element.first, element.second, element.third))
    }

    return result
}
```

O(n^2)이 걸린다. TLE뜨겠네.. 하면서 돌렸는데 됐고 discussion보니까 O(n^2)이 애초에 최적의 솔루션이라서 다행이었던 하루!!