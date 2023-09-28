# 206. Reverse Linked List (Easy)

<figure><img src="../../../../.gitbook/assets/image (63).png" alt="" width="375"><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/image (71).png" alt="" width="317"><figcaption></figcaption></figure>

### 方法一：Recursion

#### 不要试图跳进recursion，我们的小脑袋能塞下几个栈【doge】？

理解Recursion很重要的一点：明确recursion函数的含义！

**定义我们的reverse函数**：输入一个节点head，将【以head为起点的链表】反转，并且返回值是反转后的头节点。

```java
public ListNode reverseList(ListNode head) {
    // base case
    if (head == null || head.next == null) {
        return head;
    }
    // the recursion function returns a reversed list whose head node is "last"
    ListNode last = reverseList(head.next);
    head.next.next = head;
    head.next = null;

    return last;
}
```

![](<../../../../.gitbook/assets/image (82).png>)![](<../../../../.gitbook/assets/image (103).png>)

![](<../../../../.gitbook/assets/image (127).png>)![](<../../../../.gitbook/assets/image (90).png>)

### 时间复杂度

**O(N)**，遍历了一遍链表

### 空间复杂度

**O(N)**，function call stack space



### 方法二：iterative

Keep track of three nodes: **prev, curr, and next;**

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

### 时间复杂度

**O(N)**，遍历了一遍链表

### 空间复杂度

**O(1)**，only pointers xD
