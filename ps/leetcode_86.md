# 86. Partition Lists

[Link](https://leetcode.com/problems/partition-list/)

```java
public ListNode partition(ListNode head, int x) {
    final ListNode firstHalfBuffer = new ListNode(0);
    final ListNode secondHalfBuffer = new ListNode(0);
    ListNode curFirstHalf = firstHalfBuffer;
    ListNode curSecondHalf = secondHalfBuffer;
    ListNode cur = head;

    while (cur != null) {
        if (cur.val < x) {
            curFirstHalf.next = cur;
            curFirstHalf = curFirstHalf.next;
        } else {
            curSecondHalf.next = cur;
            curSecondHalf = curSecondHalf.next;
        }

        cur = cur.next;
    }

    curSecondHalf.next = null;
    curFirstHalf.next = secondHalfBuffer.next;

    return firstHalfBuffer.next;
}
```

타임 O(n), 스페이스 O(n)!!

쉽게 풀었다!!