# Deque/Stack/Queue

<figure><img src="../.gitbook/assets/image (103).png" alt="" width="346"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (64).png" alt="" width="320"><figcaption></figcaption></figure>

**为什么不推荐使用Stack？**&#x20;

因为Vector是当初Java曾经写得不太行的类，所以Stack也不太行。

Vector不行是因为效率不太行，很多方法都用了synchronized修饰，虽然线程安全，但是像ArrayDeque, LinkedList 这些线程不安全的，在需要安全的时候也可以用Collections.synchronizedCollection()转化成线程安全的，所以Vector就没什么用处了。

再根据仿生学 Stack只能上进上出，有点像刺胞动物（腔肠动物），就是那种从哪里吃进去就哪里拉出来的那种生活在海洋里的比较低级的生物。 Deque上进上出，上进下出，甚至下进上出，非常上流，只有你想不到，没有我Deque做不到的。

**Example**：

```java
Deque<TreeNode> stack = new LinkedList<>();
Deque<TreeNode> stack = new ArrayDeque<>();
```

<figure><img src="../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

### Queue

<figure><img src="../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>

### Deque方法

<figure><img src="../.gitbook/assets/image (130).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>
