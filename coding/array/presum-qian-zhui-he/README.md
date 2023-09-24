# PreSum前缀和

**前缀和主要适用的场景是：**

1. **原始数组不会被修改的情况下**
2. **频繁查询某个区间的累加和**

### Example Scenario:

这个技巧在生活中运用也挺广泛的，比方说，你们班上有若干同学，每个同学有一个期末考试的成绩（满分 100 分），那么请你实现一个 API，输入任意一个分数段，**返回有多少同学的成绩在这个分数段内**。

那么，你可以先通过**计数排序**的方式计算**每个分数具体有多少个同学**，然后利用前缀和技巧来实现分数段查询的 API：

#### 思路：

“输入任意一个分数段，**返回有多少同学的成绩在这个分数段” -> create a count array and calculate the number of each score -> sum for a range of score**&#x20;

```java
int[] scores;
int[] count = new int[100 + 1]; // full score of 100
for (int score: scores) {
    count[score] += 1;
}

for (int i = 1; i < count.length; i++) {
    count[i] = count[i] + count[i - 1];
}

```

**有多少同学的成绩在分数段（i, j）：count\[j + 1] - count\[i]**
