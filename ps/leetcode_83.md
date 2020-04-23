# 83. Remove Duplicates from Sorted List

[Link](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)

```java
public ListNode deleteDuplicates(ListNode head) {
    if (head == null) {
        return head;
    }

    ListNode cur = head;
    ListNode next = cur.next;

    while (next != null) {
        if (cur.val == next.val) {
            next = next.next;
        } else {
            cur.next = next;
            cur = next;
            next = cur.next;
        }
    }
    cur.next = null;

    return head;
}
```

O(n)!!! 쉬운 문제였다!!!!!! 

오늘은 피곤한 하루였다!! 문제가 쉬워서 다행이다 :)

!!!!!!!!!!!!!!