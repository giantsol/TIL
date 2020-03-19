# 23. Merge k Sorted Lists

[Link](https://leetcode.com/problems/merge-k-sorted-lists/)

```kotlin
fun mergeKLists(lists: Array<ListNode?>): ListNode? {
    val minHeap = PriorityQueue<ListNode> { o1, o2 -> o1.`val` - o2.`val` }
    lists.forEach {
        if (it != null) {
            minHeap.add(it)
        }
    }

    val buffer = ListNode(0)
    var cur = buffer
    while (minHeap.isNotEmpty()) {
        val node = minHeap.poll()
        cur.next = node
        if (node.next != null) {
            minHeap.add(node.next)
        }
        cur = cur.next!!
    }

    return buffer.next
}
```

MinHeap을 사용해서 O(nlogk)!

n은 모든 엘리먼트 갯수, k는 lists의 길이.