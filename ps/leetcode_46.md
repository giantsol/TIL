# 46. Permutations

[Link](https://leetcode.com/problems/permutations/)

```kotlin
fun permute(array: IntArray): List<List<Int>> {
    val permutations = arrayListOf<List<Int>>()
    loop(array.toList(), 0, permutations)
    return permutations
}

private fun loop(list: List<Int>, start: Int, permutations: ArrayList<List<Int>>) {
    if (start == list.size - 1) {
        permutations.add(list)
        return
    }

    for (i in start until list.size) {
        loop(list.swapped(start, i), start + 1, permutations)
    }
}

private fun List<Int>.swapped(i: Int, j: Int): List<Int> {
    val copy = mutableListOf<Int>()
    for (index in 0 until size) {
        when (index) {
            i -> copy.add(get(j))
            j -> copy.add(get(i))
            else -> copy.add(get(index))
        }
    }
    return copy
}
```

이런거 너무 어려운거 아냐? ㅋㅋㅋㅋㅋㅋ 에라잇! 졸리다!
