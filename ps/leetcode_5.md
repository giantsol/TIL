#5. Longest Palindromic Substring

[Link](https://leetcode.com/problems/longest-palindromic-substring/)

```kotlin
fun longestPalindrome(s: String): String {
    if (s.isEmpty()) {
        return ""
    }

    var result = s[0].toString()
    for (i in 1 until s.length) {
        val palindrome = getPalindrome(s, i)
        if (palindrome.length == s.length) {
            return s
        } else {
            result = if (result.length > palindrome.length) {
                result
            } else {
                palindrome
            }
        }
    }

    return result
}

private fun getPalindrome(s: String, i: Int): String {
    var allSameValue: Char? = s[i]
    var leftP = i - 1
    var rightP = i + 1
    val sb = StringBuilder()
    sb.append(allSameValue)

    while (leftP >= 0 || rightP < s.length) {
        val left = s.getOrNull(leftP)
        val right = s.getOrNull(rightP)
        if (left == right) {
            sb.insert(0, left)
            sb.append(right)
            if (left != allSameValue) {
                allSameValue = null
            }
        } else {
            if (allSameValue != null) {
                if (left == allSameValue) {
                    rightP -= 1
                    sb.insert(0, left)
                } else if (right == allSameValue) {
                    leftP += 1
                    sb.append(right)
                } else {
                    return sb.toString()
                }
            } else {
                return sb.toString()
            }
        }

        leftP -= 1
        rightP += 1
    }

    return sb.toString()
}
```

코드가 길고 복잡한데!! 어쩔수없지! 애초에 O(n^2)이 최적이라고 한다!!
이런 경우 보통 space를 좀 사용해서 최적화시킬 수 있을지도 모르는데 생각 안해볼것이다!

Solution보니까 O(n)에 풀리는 Manacher's algorithm이라는게 있다..! 안읽어봤지만 이게 예전에 텟쓰가 말했던건가보다!!