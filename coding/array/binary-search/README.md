# Binary Search

### If there is a monotonic relationship in array（单调关系）, consider using Binary Search!

1\. Find a **Target**

2\. **Exclude** the range where the target is not possible to live in

There are 3 different types of scenarios:

* Find the **target**
* Find the **left boundary**
* Find the **right boundary**

3\. Determine the conditions under which **nums\[mid]** might be a potential result and when it cannot be. Depending on these conditions, we will decide whether to continue the search to the left or right.

* 看到 `mid` 的时候一定要清楚两件事情：

1. `mid` 是不是解；
2. 下一轮向左边找，还是向右边找。

<figure><img src="../../../.gitbook/assets/image (141).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (142).png" alt=""><figcaption></figcaption></figure>

#### Template 1: while (left <= right)

<figure><img src="../../../.gitbook/assets/image (151).png" alt=""><figcaption></figcaption></figure>

#### Template 2: while (left < right)

It exits the while loop when **left == right**

* 会有
  * 当nums\[mid] = target，我们需要移动left pointer的时候：「`left = mid` 与 `right = mid - 1`」：
    * 可以写作：「`left = mid` 与 `right = mid - 1`」：这种写法最难理解的地方是当看到 `left = mid` 的时候，取中间数需要加 1。原因在于：**整数除法是下取整，取 `mid` 的时候不能做到真正取到中间位置**，例如 `left = 3, right = 4`， `mid = (left + right) / 2 = 3`，此时 `mid` 的值等于 `left`，一旦进入 `left = mid` 这个分支，搜索区间不能缩小，因此会进入死循环。
    * 也可以：加一个ans = mid，![](<../../../.gitbook/assets/image (23).png>)
  * 与「`left = mid + 1` 与 `right = mid`」这两种区间设置，其实就是一个包含 `mid` 一个不包含 `mid` 的区别而已。

<figure><img src="../../../.gitbook/assets/image (150).png" alt=""><figcaption></figcaption></figure>

### 总结

**当搜索区间 \[left..right] 里只有 2 个元素的时候**：

1. 如果划分区间的逻辑是 `left = mid + 1`; 和 `right = mid`时，while(left < right) 退出循环以后 left == right 成立，此时 mid 中间数正常下取整就好；&#x20;
2. 如果划分区间的逻辑是 `left = mid`; 和 `right = mid - 1` 时，while(left < right) 退出循环以后 left == right 成立，此时为了避免死循环，mid 中间数需要改成上取整。



### Questions that are essential to understand binary search:

{% content-ref url="34.-find-first-and-last-position-of-element-in-sorted-array-medium.md" %}
[34.-find-first-and-last-position-of-element-in-sorted-array-medium.md](34.-find-first-and-last-position-of-element-in-sorted-array-medium.md)
{% endcontent-ref %}

{% content-ref url="35.-search-insert-position-easy.md" %}
[35.-search-insert-position-easy.md](35.-search-insert-position-easy.md)
{% endcontent-ref %}

{% content-ref url="69.-sqrt-x-easy.md" %}
[69.-sqrt-x-easy.md](69.-sqrt-x-easy.md)
{% endcontent-ref %}
