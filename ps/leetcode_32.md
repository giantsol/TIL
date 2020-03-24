# 32. Longest Valid Parentheses

[Link](https://leetcode.com/problems/longest-valid-parentheses/)

```kotlin
fun longestValidParentheses(s: String): Int {
    val list = arrayListOf(0)
    var maxLen = 0
    var num = 0

    for (char in s) {
        if (char == '(') {
            num++
        } else {
            num--
        }
        list.add(num)
    }

    val map = hashMapOf<Int, Int>()
    for (i in list.indices) {
        num = list[i]
        val nextNum = list.getOrElse(i + 1) { num }
        if (num in map) {
            maxLen = max(maxLen, i - map[num]!!)
        } else if (nextNum > num) {
            map[num] = i
        }

        if (nextNum < num) {
            map.remove(num)
        }
    }

    return maxLen
}
```

time과 space complexity 모두 O(n)!

')'일때는 -1, '('일때는 +1 식으로 해서 '(()'를 [0, 1, 2, 1] 식으로 바꾼 다음에 같은 숫자들을 매칭하면 된다.

뭔가 요소가 딱 2개일 때 이런 식의 접근이 자주 쓰이는듯!!