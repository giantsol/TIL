# 24. Swap Nodes in Pairs

[Link](https://leetcode.com/problems/swap-nodes-in-pairs/)

```kotlin
fun swapPairs(head: ListNode?): ListNode? {
    if (head == null) {
        return null
    }

    val buffer = ListNode(0).apply { next = head }
    var cur: ListNode? = head
    var next: ListNode? = head.next
    var prev: ListNode? = buffer

    while (next != null) {
        var tmp = next?.next
        prev?.next = next
        next?.next = cur
        cur?.next = tmp

        //swap cur and next
        tmp = cur
        cur = next
        next = tmp

        for (i in 0 until 2) {
            prev = cur
            cur = next
            next = next?.next
        }
    }

    return buffer.next
}
```

포인터 3개 가지고 요래저래 장난치면 된다. 시간은 O(n)!

이 문제였나?? 예전에 알고리즘 스터디할 때 데미안이 나한테 냈던 문제랑 비슷한거같은데..
그때는 이런 문제 못풀었을텐데 지금은 게임방송 들으면서 할 정도는 되었닼ㅋㅋㅋㅋㅋ

스터디 없으니까 조용히 문제 풀기 너무 심심하네!!!!