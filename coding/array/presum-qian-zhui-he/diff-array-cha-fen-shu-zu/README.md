# Diff Array差分数组

**差分数组的主要适用场景是频繁对原始数组的某个区间的元素进行增减**。

比如说，我给你输入一个数组 `nums`，然后又要求给区间 `nums[2..6]` 全部加 1，再给 `nums[3..9]` 全部减 3，再给 `nums[0..4]` 全部加 2，再一通操作猛如虎，然后问你，最后 `nums` 数组的值是什么？

常规的思路很容易，你让我给区间 `nums[i..j]` 加上 `val`，那我就一个 for 循环给它们都加上呗，还能咋样？这种思路的时间复杂度是 O(N)，由于**这个场景下对 `nums` 的修改非常频繁，所以效率会很低下**。

这里就需要差分数组的技巧，类似前缀和技巧构造的 `preSum` 数组，我们先对 `nums` 数组构造一个 `diff` 差分数组，**`diff[i]` 就是 `nums[i]` 和 `nums[i-1]` 之差**：

```java
int[] diff = new int[nums.length];
diff[0] = nums[0];

for (int i = 1; i < nums.length; i++) {
    diff[i] = nums[i] - nums[i - 1];
}
```

<figure><img src="../../../../.gitbook/assets/image (50).png" alt="" width="563"><figcaption></figcaption></figure>

由diff\[i]反推出nums\[i]：

```java
int[] res = new int[diff.length];
res[0] = diff[0];

for (int i = 1; i < diff.length; i++) {
    res[i] = res[i - 1] + diff[i];
]
```

<figure><img src="../../../../.gitbook/assets/image (51).png" alt="" width="563"><figcaption></figcaption></figure>

**这样构造差分数组 `diff`，就可以快速进行区间增减的操作**，如果我们想对区间 `nums[i..j]` 的元素全部加 3，那么只需要让 `diff[i] += 3`，然后再让 `diff[j+1] -= 3` 即可：

**原理很简单，回想 `diff` 数组反推 `nums` 数组的过程，`diff[i] += 3` 意味着给 `nums[i..]` 所有的元素都加了 3，然后 `diff[j+1] -= 3` 又意味着对于 `nums[j+1..]` 所有元素再减 3，那综合起来，是不是就是对 `nums[i..j]` 中的所有元素都加 3 了**？

只要花费 O(1) 的时间修改 `diff` 数组，就相当于给 `nums` 的整个区间做了修改。多次修改 `diff`，然后通过 `diff` 数组反推，即可得到 `nums` 修改后的结果。

把差分数组抽象成一个类，包含 `increment` 方法和 `result` 方法：

```java
class Difference {
    // 差分数组
    private int[] diff;
    
    public Difference(int[] nums) {
        assert nums.length > 0;
        diff = new int[nums.length];
        diff[0] = nums[0];
        for (int i = 1; i < nums.length; i++) {
            diff[i] = nums[i] - nums[i - 1];
        }
    }
    
    // 给区间[i, j]增加val（val可以是负数）
    public void increment(int i, int j, int val) {
        diff[i] += val;
        if (j + 1 < diff.length) {
            diff[j + 1] -= val;
        }
    }

    public int[] result() {
        int[] res = new int[diff.length];
        res[0] = diff[0];
        for (int i = 1; i < diff.length; i++) {
            res[i] = res[i - 1] + diff[i];
        }
        return res;
    }
}
```

为什么**increment**的方法里要判断**`j + 1`**是否小于**`diff.length`，因为**当 `j+1 >= diff.length` 时，说明是对 `nums[i]` 及以后的整个数组都进行修改，那么就不需要再给 `diff` 数组减 `val` 了。

\
