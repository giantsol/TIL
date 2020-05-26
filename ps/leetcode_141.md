# 141. Linked List Cycle

[Link](https://leetcode.com/problems/linked-list-cycle/)

```java
public boolean hasCycle(ListNode head) {
    if (head == null) {
        return false;
    }

    ListNode fast = head;
    ListNode slow = head;

    while (fast != null) {
        fast = fast.next;
        if (fast == null) {
            break;
        } else {
            fast = fast.next;
        }
        if (fast == slow) {
            return true;
        }

        slow = slow.next;
        if (fast == slow) {
            return true;
        }
    }

    return false;
}
```

빠른애와 느린애 사용해서 해결할 수 있는 문제입니다!!