# Dynamic Programming

1. 「无后效性」的设计思想：让不确定的因素确定下来，以保证求解的过程形成一个逻辑上的有向无环图。
   * 容易有后效性的设计方式：53. Maximum Subarray, 300. Longest Increasing Subsequence

{% content-ref url="53.-maximum-subarray-medium.md" %}
[53.-maximum-subarray-medium.md](53.-maximum-subarray-medium.md)
{% endcontent-ref %}

{% content-ref url="300.-longest-increasing-subsequence.md" %}
[300.-longest-increasing-subsequence.md](300.-longest-increasing-subsequence.md)
{% endcontent-ref %}

2. 动态规划的题目分为两大类，它们都存在一定的**递推性质**:
   1. 一种是求**最优解**类，典型问题是背包问题: 这类问题的递推性质还有一个名字，叫做 「最优子结构」 ——即当前问题的最优解取决于子问题的最优解，
   2. 另一种就是**计数类**，比如这里的统计方案数（62. Unique Paths）的问题，这类问题的递推性质：当前问题的方案数取决于子问题的方案数。所以在遇到求方案数的问题时，我们可以往动态规划的方向考虑。
3. 我们可以运用「**滚动数组思想**」把空间复杂度优化称 **O(m)**。「滚动数组思想」是一种常见的动态规划优化方法，在我们的题目中已经多次使用到，例如「剑指 Offer 46. 把数字翻译成字符串」、「70. 爬楼梯」等，当我们定义的**状态在动态规划的转移方程中只和某几个状态相关**的时候，就可以考虑这种优化方法，目的是给空间复杂度「降维」。
4. 在很多教程中都会写，动态规划有两种计算顺序，一种是自顶向下的、使用**备忘录**的递归方法，**一种是自底向上的、使用 dp 数组的循环方法**。不过在普通的动态规划题目中，99% 的情况我们都不需要用到备忘录方法，所以我们最好坚持用**自底向上的 dp 数组**。
