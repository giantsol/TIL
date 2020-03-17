# 19. Remove Nth Node From End of List

[Link](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

```kotlin
val buffer = ListNode(0).apply { next = head }
var prev: ListNode? = buffer
var cur = head
var next = head?.next
var temp = cur
var nc = n
while (nc > 0) {
    temp = temp?.next
    nc -= 1
}

while (temp != null) {
    temp = temp.next
    prev = cur
    cur = next
    next = next?.next
}

prev?.next = next
return buffer.next
```

O(n)!

LinkedList 앞에 buffer를 하나 두는건 데미안에게서 처음 배웠던 아주 유용한 꼼수!!