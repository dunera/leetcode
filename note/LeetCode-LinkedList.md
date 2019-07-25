### [LeetCode-24 (Medium)](https://leetcode.com/problems/swap-nodes-in-pairs/)	**Swap Nodes in Pairs**

题意：给定一个链表，交换链表中相邻的两个节点，只允许修改节点之间关联，而不允许修改节点的值。

示例：

```
Given 1->2->3->4, you should return the list as 2->1->4->3.
```

**解法一：**

```java
public static ListNode swapPairs(ListNode head) {
    ListNode node = new ListNode(-1);
    node.next = head;
    ListNode pre = node;
    while (pre.next != null && pre.next.next != null) {
        ListNode l1 = pre.next, l2 = pre.next.next;
        l1.next = l2.next;
        l2.next = l1;
        pre.next = l2;
        pre = l1;
    }
    return node.next;
}
```

*时间复杂度：O(N)*

*空间复杂度：O(1)*

**解法二：**递归

```java
public ListNode swapPairs(ListNode head) {
    if ((head == null) || (head.next == null))
        return head;
    ListNode n = head.next;
    head.next = swapPairs(head.next.next);
    n.next = head;
    return n;
}
```

----

