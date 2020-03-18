# 21. Merge Two Sorted Lists

[Link](https://leetcode.com/problems/merge-two-sorted-lists/)

```kotlin
val head = ListNode(0)
var cur = head
var first = l1
var second = l2
while (first != null || second != null) {
    if (second == null) {
        cur.next = first
        first = first!!.next
    } else if (first == null) {
        cur.next = second
        second = second.next
    } else {
        if (first.`val` <= second.`val`) {
            cur.next = first
            first = first.next
        } else {
            cur.next = second
            second = second.next
        }
    }

    cur = cur.next!!
}

return head.next
```

O(n)!
쉬운 문제!