# 11. Container With Most Water

[Link](https://leetcode.com/problems/container-with-most-water/)

```kotlin
fun maxArea(height: IntArray): Int {
    var res = 0
    for (i in 0 until height.size - 1) {
        for (j in (i + 1) until height.size) {
            res = max(res, (j - i) * min(height[i], height[j]))
        }
    }

    return res
}
```

ㅋ.ㅋ!! 그냥 하나씩 다 해봐서 O(n^2)!!
이래도 Time Limit Exceeded 안떠서 그냥 ㅎ.ㅎ..
Solution 보니까 포인터 2개 써서 O(n)으로 할 수 있더라!!