# 16. 3Sum Closet

```kotlin
fun threeSumClosest(nums: IntArray, target: Int): Int {
    nums.sort()
    var res = nums[0] + nums[1] + nums[2]
    var minDiff = abs(target - res)
    for (i in 0 until nums.size - 2) {
        val num = nums[i]
        var start = i + 1
        var end = nums.size - 1
        while (end > start) {
            val sum = num + nums[start] + nums[end]
            val diff = abs(target - sum)
            if (diff < minDiff) {
                res = sum
                minDiff = diff
            }
            if (sum < target) {
                start += 1
            } else {
                end -= 1
            }
        }
    }
    
    return res
}
```

O(n^2)!!
호오.. 세번 틀렸다가 성공했는데 첫 세번 동안 완전 접근을 잘못했었다!!
포인터 3개 써서 해결하는 방식!!
이게 최적이라고 한다!
