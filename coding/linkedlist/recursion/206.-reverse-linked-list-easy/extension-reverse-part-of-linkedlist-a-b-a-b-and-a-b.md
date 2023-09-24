# Extension: Reverse part of LinkedList \[a, b), \[a, b], and (a,b)

### From 206: we are reversing the entire linked list (from the head node to null), can we reverse part of the LinkedList, including three situations, \[a, b), \[a, b], and (a, b)?

**YES!**



### Reverse the entire LinkedList:

```java
public ListNode reverseList(ListNode head) {
    // Solution 2: iterative
    ListNode prev = null;
    ListNode curr = head;

    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

### \[a, b)

**NOTE: 注意此函数只反转\[a, b)区间的链表，并没有把前后的nodes连接起来！**

```java
public ListNode reverseList(ListNode a, ListNode b) {
    ListNode prev = null;
    ListNode curr = a;
    ListNode next = a;
    
    while (curr != b) { // 把while的终止条件从curr!=null改为b即可）
        next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    // 返回reverse后的头节点（即prev）
    return prev;
}
```

### \[a, b]

92. [Reverse Linked List II (Medium)](../92.-reverse-linked-list-ii-medium.md)

{% code overflow="wrap" %}
```java
public ListNode reverseBetween(ListNode head, int left, int right) {
    if (head.next == null) {
        return head;
    }
    ListNode dummy = new ListNode(-1);
    dummy.next = head;
    ListNode prev = dummy;
    for (int i = 0; i < left - 1; i++) {
        prev = prev.next;
    }

    ListNode curr = prev.next;
    for (int i = 0; i < right - left; i++) {
        ListNode forw = curr.next;
        curr.next = forw.next;
        forw.next = prev.next; // Use prev.next instead of curr because prev.next always points to the already-reversed sub-linkedlist's head
        prev.next = forw;
    }

    return dummy.next;
}
```
{% endcode %}

### (a, b)

Use "**begin**" to keep track of the previous node of the start node, and "**first**" to keep track of the start node;

**NOTE: 此函数有把反转区间的前后链表连接起来 ：）**

<pre class="language-java" data-overflow="wrap"><code class="lang-java">public ListNode reverse(ListNode begin, ListNode end) {
    ListNode curr = begin.next;
<strong>    ListNode first = curr;
</strong>    ListNode prev = begin;
    ListNode next = null;
    
    while (curr != end) {
        next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    // when outside of the while loop, curr = end（connect the linkedlist's tail with the remaining linked list）
    first.next = curr;
    // connect the front linkedlist to the newly reversed linkedlist's head (which is prev)
    begin.next = prev;
    
    return first;
}
</code></pre>
