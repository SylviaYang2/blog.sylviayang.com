# TikTok OA

题目描述：

给你一个长度为 2n 的数组 data，data\[i] 表示一台服务器的 capacity。 其中每两台服务器可以凑成一对，现在要关闭每一对中的一台服务器。      关闭其中 capacity 小的服务器，会使 cost 加 1，关闭大的没有 cost。

给你数组 data 和 int k，要求返回在关闭服务器后，还在运行的服务器的 capacity 之和大于等于 k 时的 最小 cost。

data = \[1, 2, 3, 5, 7, 8]    k = 14 result: \[1, 8] \[2, 3] \[5, 7] 关闭 1, 3, 7。 cost = 1



cost = 0 的最优组合情况。1.如果服务器的capacity不存在相同的直接相邻的组合(排序后)。这样关闭所有capacity大依然能够使剩下的服务器capacity保持最大。 2. 如果服务器的capacity存在相同的，单调栈维护右边第一个比其大且未和其他服务器组合的服务器组合。

cost != 0 即 cost=\[1,n]。此时意味着关闭服务器capacity小的服务器，要使剩下的capacity最大，就使用capacity最大且未被组合的服务器和其组合，这样关闭小的保留最大的依然能够使得剩下的capacity最大。

这个问题的解法涉及到贪心算法和二分查找。我将逐步解释代码的主要部分。

首先，代码对服务器的容量进行排序，然后使用一个数组`preSum`来维护前缀和，以便在后续计算中更方便地获取服务器容量之和。

```java
Arrays.sort(capacity);
int sumCapacity = 0;
int[] preSum = new int[capacity.length + 1];
for (int i = 1; i <= capacity.length; i++) {
    sumCapacity += capacity[i - 1];
    preSum[i] = sumCapacity;
}
```

接下来，通过二分查找来确定最小的cost。在二分的过程中，使用`closeMax`方法来模拟关闭大容量服务器的情况。这个方法使用单调栈维护右边第一个比当前服务器容量大且未被组合的服务器，以确保关闭最大的服务器。

```java
int l = 0, r = capacity.length / 2;
while (l <= r) {
    int mid = l + (r - l >> 1);
    if (sumCapacity - closeMax(mid, capacity, preSum[mid]) >= k) {
        r = mid - 1;
    } else {
        l = mid + 1;
    }
}
return l;
```

最后，`closeMax`方法实现了关闭大容量服务器的逻辑。通过使用单调栈，该方法找到右边第一个比当前服务器容量大且未被组合的服务器，模拟关闭大容量服务器的操作。

```java
public int closeMax(int cost, int[] capacity, int close) {
    ArrayDeque<Integer> stack = new ArrayDeque<>();
    for (int i = capacity.length - 1 - cost; i >= cost; i--) {
        boolean pop = false;
        if (!stack.isEmpty() && stack.peek() > capacity[i]) {
            close += stack.pop();
            pop = true;
        }
        if (!pop) {
            stack.push(capacity[i]);
        }
    }
    return close;
}
```

总体而言，这个解法通过贪心地关闭大容量服务器，并通过二分查找确定最小的cost，以实现在关闭服务器后，还在运行的服务器的容量之和大于等于k时的最小cost。



**Complexity Analysis**

* Time complexity: O(nlogn)
* Space complexity: O(n)

