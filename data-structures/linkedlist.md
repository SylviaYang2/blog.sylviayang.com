# LinkedList

### Java Implementation

```java
public class ListNode {
    public int val;
    public ListNode next;
    public ListNode(int val) {
        this.val = val;
        this.next = null;
    }
}
```

### LinkedList Robustness

当访问curr或者curr.next时，需要结合情况判断curr与curr.next是否为null

### Dummy Node

Dummy Node一般指向head，dummy -> head，防止head node丢失；

即当linkedlist的head有可能变化（被修改或者删除时），可以使用dummy node，最终返回的dummy -> head即为新的链表。

### LinkedList 基本操作

1. Reverse 反转链表

```java
1 -> 2 -> 3 -> null   -->   3 -> 2 -> 1 -> null

         1   ->   2   ->   3   ->  null
prev    head     next

// iterative
public ListNode reverse(ListNode head) {
     ListNode prev = null;
     while (head != null) {
          ListNode next = head.next;
          head.next = prev;
          prev = head;
          head = next;
     }
     return prev;
}

// recursive
public ListNode reverse(ListNode head) {
     if (head == null || head.next == null) {
          return head;
     }
     
     ListNode next = head.next;
     // preorder位置处理recursion
     ListNode newHead = reverse(head);
     next.next = head;
     head.next = null;
     
     return newHead;
}
```

2. 双向链表

双向链表的reverse的重点：

* curr.prev和curr.next的交换
* head的递推（curr相当于head的dummy node）

```java
class DListNode {
    int val;
    DListNode prev, next;
    DListNode(int val) {
        this.val = val;
        this.prev = this.next = null;
    }
}


public DListNode reverse(DListNode head) {
    DListNode curr = null;
    while (head != null) {
        curr = head;
        head = curr.next;
        curr.next = curr.prev;
        curr.prev = head;
    }
    return curr;
}
```

### 快慢指针

fast和slow都指向head node，fast的移动速度是slow的2倍

应用场景：

* 找未知长度的linkedlist中间节点：
  * 偶数长度：如果fast=null时，slow刚好在链表的中间两个节点的后者
  * 奇数长度：如果fast=null时，slow在链表中间节点
* 判断linkedlist是否有环：如果fast=null，则此链表无环；如果fast=slow，则快指针追上慢指针，此链表有环。

```java
public boolean hasCircle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    
    while (slow != null && fast != null) {
        slow = slow.next;
        fast = fast.next;
        if (fast != null) {
            fast = fast.next;
        }
        if (fast == slow) {
            break;
        }
    }
    if (fast != null && slow != null && fast == slow) {
        return true;
    }
    return false;
}
```

