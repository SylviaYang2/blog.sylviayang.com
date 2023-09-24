# 反转链表前 N 个节点 Reverse the First N Nodes

### Signature：

将链表的前N个节点反转（N <= 链表长度）

`ListNode reverseN(ListNode head, int n)`

<figure><img src="../../../.gitbook/assets/image (100).png" alt="" width="375"><figcaption></figcaption></figure>

### 思路：

1. base case变为：当n == 1时（即到原链表的头节点时），要记录原链表第N个节点的后驱节点（successor）；
2. head.next要设置成第N+1个节点；

```java
public ListNode reverseN(ListNode head, int n) {
    // 后驱节点
    ListNode successor = null;
    // base case
    if (n == 1) {
        ListNode successor = head.next;
        return head;
    }
    
    // 以head.next为起点，反转前n-1个节点
    ListNode last = reverseN(head.next, n - 1);
    successor = head.next.next;
    head.next.next = head;
    head.next = successor;
    
    return last;
}
```

