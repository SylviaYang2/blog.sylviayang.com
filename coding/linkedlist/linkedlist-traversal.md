# LinkedList Traversal

### Tree其实可以看做是LinkedList的衍生（extension），无非就是多条LinkedList的一个数据结构；

LinkedList也可以有preorder & postorder traversal：

```java
void traverse(ListNode head) {
    // preorder
    traverse(head.next);
    // postorder
}
```

Print val in order:

```java
void preorder(ListNode head) {
    if (head == null) {
        return;
    }
    System.out.print(head.val);
    traverse(head.next);
}
```

Print val in **reverse** order:

```java
void postorder(ListNode head) {
    if (head == null) {
        return;
    }
    traverse(head.next);
    System.out.print(head.val);
}
```
