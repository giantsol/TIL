# 25. Reverse Nodes in k-Group

[Link](https://leetcode.com/problems/reverse-nodes-in-k-group/)

```kotlin
fun reverseKGroup(head: ListNode?, k: Int): ListNode? {
    val buffer = ListNode(0).apply { next = head }
    var prev = buffer
    var start = head
    var end = start
    for (i in 0 until k - 1) {
        end = end?.next
    }

    while (end != null) {
        prev.next = end
        val tmp = end.next
        prev = reverse(start!!, end)
        start.next = tmp

        start = prev.next
        end = start
        for (i in 0 until k - 1) {
            end = end?.next
        }
    }

    return buffer.next
}

private fun reverse(start: ListNode, end: ListNode): ListNode {
    var c: ListNode? = start
    var n = c?.next
    while (c != end) {
        val tmp = n?.next
        n?.next = c
        c = n
        n = tmp
    }
    return start
}
```

흠냐 코드는 더럽지만 별로 정리할 생각은 없다!! 일단은 풀었으면 됐지!
O(n)이 걸린다!
