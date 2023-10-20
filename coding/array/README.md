# Array

1. **Interview Tip**: Whenever you're trying to solve an array problem **in-place**, always consider the possibility of iterating **backwards** instead of forwards through the array. It can completely change the problem, and make it a lot easier.
2. A lot of these problems with **subarrays equal to / greater than / less than a target** generally use one of 1 strategies for the optimal solution -&#x20;

* **sliding window** with 2 pointers&#x20;
  * given list is **all** positive or negative
    * Actually 53. can be solved using sliding window
  *   想用滑动窗口算法，先问自己几个问题：

      1、什么时候应该扩大窗口？

      2、什么时候应该缩小窗口？

      3、什么时候更新答案？

{% content-ref url="dynamic-programming/53.-maximum-subarray-medium.md" %}
[53.-maximum-subarray-medium.md](dynamic-programming/53.-maximum-subarray-medium.md)
{% endcontent-ref %}

* no restriction on the number of target elements (e.g. 525)

{% content-ref url="prefix-sum-qian-zhui-he/prefix-sum-+-hashmap/525.-contiguous-array-medium.md" %}
[525.-contiguous-array-medium.md](prefix-sum-qian-zhui-he/prefix-sum-+-hashmap/525.-contiguous-array-medium.md)
{% endcontent-ref %}

* **hashmap** storing cumulative sum (**prefix sum**)
  * if  don't know the sign of the numbers, better to use a hashmap since you cannot anticipate what expanding/contracting a window might do

3. 通常我们遍历子串或者子序列有三种遍历方式:

* 以某个节点为开头的所有子序列: 如 \[a]，\[a, b]，\[ a, b, c] ... 再从以 b 为开头的子序列开始遍历 \[b] \[b, c]。&#x20;
* 根据子序列的长度为标杆，如先遍历出子序列长度为 1 的子序列，在遍历出长度为 2 的 等等。
* 以子序列的结束节点为基准，先遍历出以某个节点为结束的所有子序列，因为每个节点都可能会是子序列的结束节点，因此要遍历下整个序列，如: 以 b 为结束点的所有子序列: \[a , b] \[b]。以 c 为结束点的所有子序列: \[a, b, c] \[b, c] \[ c ]。
*   第一种遍历方式通常用于暴力解法, 第二种遍历方式 leetcode (5. 最长回文子串 ) 中的解法就用到了。第三种遍历方式 因为可以产生递推关系, 采用**DP**时, 经常通过此种遍历方式, 如：背包问题, 最大公共子串...





