# 17. Letter Combinations of a Phone Number

[Link](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

```kotlin
fun letterCombinations(digits: String): List<String> {
    if (digits.isEmpty()) {
        return emptyList()
    }

    val queue = LinkedList<String>()
    val mappings = listOf(charArrayOf(), charArrayOf(),
        charArrayOf('a', 'b', 'c'),
        charArrayOf('d', 'e', 'f'),
        charArrayOf('g', 'h', 'i'),
        charArrayOf('j', 'k', 'l'),
        charArrayOf('m', 'n', 'o'),
        charArrayOf('p', 'q', 'r', 's'),
        charArrayOf('t', 'u', 'v'),
        charArrayOf('w', 'x', 'y', 'z')
        )

    queue.add("")
    for (i in digits.indices) {
        val chars = mappings[Integer.parseInt("${digits[i]}")]
        while (queue.peek().length == i) {
            val removed = queue.remove()
            for (char in chars) {
                queue.add(removed + char)
            }
        }
    }

    return queue
}
```

앗!! discussion 보고 풀었다!!
산책하면서 보던 문제인데 머리로 굴릴때 아무리생각해도 O(4^n) 밖에 생각나지 않아서 대체 최적의 솔루션은
time complexity가 어떻게 될까? 하면서 discussion을 봤을떄 O(4^n)이 최적의 솔루션이었다.

그건 둘째치고 어떻게 구현할지는 아직 생각하지 않았었는데 discussion을 보는 바람에 사실상 그대로 가져온셈!!
하지만 좋은 방법을 하나 또 배웠다 ^__^!!