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

#### Template 1: while (left <= right)

<figure><img src="../../../.gitbook/assets/image (151).png" alt=""><figcaption></figcaption></figure>

#### Template 2: while (left < right)

It exits the while loop when **left == right**

* 会有「`left = mid` 与 `right = mid - 1`」与「`left = mid + 1` 与 `right = mid`」这两种区间设置，其实就是一个包含 `mid` 一个不包含 `mid` 的区别而已。
* 这种写法最难理解的地方是当看到 `left = mid` 的时候，取中间数需要加 1。原因在于：**整数除法是下取整，取 `mid` 的时候不能做到真正取到中间位置**，例如 `left = 3, right = 4`， `mid = (left + right) / 2 = 3`，此时 `mid` 的值等于 `left`，一旦进入 `left = mid` 这个分支，搜索区间不能缩小，因此会进入死循环。

<figure><img src="../../../.gitbook/assets/image (141).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (142).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (150).png" alt=""><figcaption></figcaption></figure>

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
