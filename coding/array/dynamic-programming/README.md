# Dynamic Programming

<details>

<summary>背包问题（Knapsack）</summary>

**第一步要明确两点，「状态」和「选择」**。

先说状态，如何才能描述一个问题局面？只要给几个物品和一个背包的容量限制，就形成了一个背包问题呀。**所以状态有两个，就是「背包的容量」和「可选择的物品」**。

再说选择，也很容易想到啊，对于每件物品，你能选择什么？**选择就是「装进背包」或者「不装进背包」嘛**。

明白了状态和选择，动态规划问题基本上就解决了，只要往这个框架套就完事儿了：

```java
for 状态1 in 状态1的所有取值：
    for 状态2 in 状态2的所有取值：
        for ...
            dp[状态1][状态2][...] = 择优(选择1，选择2...)
```

&#x20;**第二步要明确 `dp` 数组的定义**。

首先看看刚才找到的「状态」，有两个，也就是说我们需要一个二维 `dp` 数组。

`dp[i][w]` 的定义如下：对于前 `i` 个物品，当前背包的容量为 `w`，这种情况下可以装的最大价值是 `dp[i][w]`。

比如说，如果 `dp[3][5] = 6`，其含义为：对于给定的一系列物品中，若只对前 3 个物品进行选择，当背包容量为 5 时，最多可以装下的价值为 6。

根据这个定义，我们想求的最终答案就是 `dp[N][W]`。base case 就是 `dp[0][..] = dp[..][0] = 0`，因为没有物品或者背包没有空间的时候，能装的最大价值就是 0。

细化上面的框架：

```java
int[][] dp[N+1][W+1]
dp[0][..] = 0
dp[..][0] = 0

for i in [1..N]:
    for w in [1..W]:
        dp[i][w] = max(
            把物品 i 装进背包,
            不把物品 i 装进背包
        )
return dp[N][W]
```

**第三步，根据「选择」，思考状态转移的逻辑**。

简单说就是，上面伪码中「把物品 `i` 装进背包」和「不把物品 `i` 装进背包」怎么用代码体现出来呢？

这就要结合对 `dp` 数组的定义，看看这两种选择会对状态产生什么影响：

先重申一下刚才我们的 `dp` 数组的定义：

`dp[i][w]` 表示：对于前 `i` 个物品（从 1 开始计数），当前背包的容量为 `w` 时，这种情况下可以装下的最大价值是 `dp[i][w]`。

**如果你没有把这第 `i` 个物品装入背包**，那么很显然，最大价值 `dp[i][w]` 应该等于 `dp[i-1][w]`，继承之前的结果。

**如果你把这第 `i` 个物品装入了背包**，那么 `dp[i][w]` 应该等于 `val[i-1] + dp[i-1][w - wt[i-1]]`。

首先，由于数组索引从 0 开始，而我们定义中的 `i` 是从 1 开始计数的，所以 `val[i-1]` 和 `wt[i-1]` 表示第 `i` 个物品的价值和重量。

你如果选择将第 `i` 个物品装进背包，那么第 `i` 个物品的价值 `val[i-1]` 肯定就到手了，接下来你就要在剩余容量 `w - wt[i-1]` 的限制下，在前 `i - 1` 个物品中挑选，求最大价值，即 `dp[i-1][w - wt[i-1]]`。

综上就是两种选择，我们都已经分析完毕，也就是写出来了状态转移方程，可以进一步细化代码：

```java
for i in [1..N]:
    for w in [1..W]:
        dp[i][w] = max(
            dp[i-1][w],
            dp[i-1][w - wt[i-1]] + val[i-1]
        )
return dp[N][W]
```

**最后一步，把伪码翻译成代码，处理一些边界情况**。

我用 Java 写的代码，把上面的思路完全翻译了一遍，并且处理了 `w - wt[i-1]` 可能小于 0 导致数组索引越界的问题：

```java
int knapsack(int W, int N, int[] wt, int[] val) {
    assert N == wt.length;
    // base case 已初始化
    int[][] dp = new int[N + 1][W + 1];
    for (int i = 1; i <= N; i++) {
        for (int w = 1; w <= W; w++) {
            if (w - wt[i - 1] < 0) {
                // 这种情况下只能选择不装入背包
                dp[i][w] = dp[i - 1][w];
            } else {
                // 装入或者不装入背包，择优
                dp[i][w] = Math.max(
                    dp[i - 1][w - wt[i-1]] + val[i-1], 
                    dp[i - 1][w]
                );
            }
        }
    }
    
    return dp[N][W];
}
```



</details>

### 背包问题：

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

纯 0 - 1 背包 是求 给定背包容量 装满背包 的最大价值是多少。

416\. 分割等和子集 是求 给定背包容量，能不能装满这个背包。&#x20;

1049\. 最后一块石头的重量 II 是求 给定背包容量，尽可能装，最多能装多少&#x20;

494\. 目标和 是求 给定背包容量，装满背包有多少种方法。&#x20;

474\. Ones and Zeroes 是求 给定背包容量，装满背包最多有多少个物品。



{% content-ref url="416.-partition-equal-subset-sum-medium.md" %}
[416.-partition-equal-subset-sum-medium.md](416.-partition-equal-subset-sum-medium.md)
{% endcontent-ref %}

{% content-ref url="../backtracking/494.-target-sum-medium.md" %}
[494.-target-sum-medium.md](../backtracking/494.-target-sum-medium.md)
{% endcontent-ref %}

{% content-ref url="../backtracking/698.-partition-to-k-equal-sum-subsets-medium.md" %}
[698.-partition-to-k-equal-sum-subsets-medium.md](../backtracking/698.-partition-to-k-equal-sum-subsets-medium.md)
{% endcontent-ref %}

{% content-ref url="474.-ones-and-zeroes-medium.md" %}
[474.-ones-and-zeroes-medium.md](474.-ones-and-zeroes-medium.md)
{% endcontent-ref %}

{% content-ref url="518.-coin-change-ii-medium.md" %}
[518.-coin-change-ii-medium.md](518.-coin-change-ii-medium.md)
{% endcontent-ref %}



### 技巧/Tricks

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
