# KMP

{% embed url="https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/solutions/732461/dai-ma-sui-xiang-lu-kmpsuan-fa-xiang-jie-mfbs/" %}

为什么left和right不匹配时left要回退到next\[left-1]，而不是left--或者直接回到0。原来是用了同样的思想，**是继续用 相同前后缀 的 相同前后缀来实现回退**。

<figure><img src="../../../.gitbook/assets/image (12).png" alt="" width="375"><figcaption></figcaption></figure>

`next` 数组存放的是当前长度下的 **最长相同前后缀** 的长度 以 `abcabf`举例:

* a时，最长前后缀长度是 0
* ab时，很显然长度也是 0
* abc时，很显然长度也是 0
* abca时， 最长相同前后缀长度就是 1了，是 a
* abcab时， 最长相同前后缀长度就是 2了，是 ab
* abcabf时，没有 最长相同前后缀了，长度是 0

可以看到，现在已经匹配到 `c`与 `f`，出现了第一个不相等的字母了

![](https://pic.leetcode-cn.com/1637734773-HUkWfu-file\_1637734773447)

*   怎么回退呢？答案是找到上一个

    > 因为**上一个肯定是已经匹配完成的字母**，我们找到它对应的 next 数组值来指导我们进行下一步操作

![](https://pic.leetcode-cn.com/1637734774-FVQUTE-file\_1637734774102)

那么问题来了，现在 next 数组值 **2** 代表什么呢？它想让我们干什么呢？

2表示当前已经匹配的子字符串 abcab的最长相同前后缀长度是 2，而我们数组下标是从 0️⃣开始的，如果我们按照 2的指示，正好可以跳转到下标 2 的位置（也就是字母C），开始下一次匹配。

<figure><img src="../../../.gitbook/assets/image (13).png" alt="" width="375"><figcaption></figcaption></figure>

### next 数组代码实现（两种写法） <a href="#id-4next-shu-zu-dai-ma-shi-xian-xi-jie-tu" id="id-4next-shu-zu-dai-ma-shi-xian-xi-jie-tu"></a>

```java
 for (int right = 1, left = 0; right < needleLength; right++) {
//    定义好两个指针right与left
//    在for循环中初始化指针right为1，left=0,开始计算next数组，right始终在left指针的后面
    while (left > 0 && needle.charAt(left) != needle.charAt(right)) {
//    如果不相等就让left指针回退，到0时就停止回退
        left = next[left - 1];//进行回退操作；
    }
    if (needle.charAt(left) == needle.charAt(right)) {
        left++;
    }
    // right pointer是从 1 开始的
    next[right] = left;
    }
// 循环结束的时候，next数组就已经计算完毕了
```

```java
while ( < m) {
    if (needle.charAt(left) == needle.charAt(right)) {
        // Length of Longest Border Increased
        left += 1;
        longest_border[right] = left;
        right += 1;
    } else {
        // Only empty border exist
        if (prev == 0) {
            longest_border[right] = 0;
            right += 1;
        }
        // Try finding longest border for this i with reduced prev
        // 如果不相等就让left指针回退，到0时就停止回退
        else {
            left = longest_border[left - 1];
        }
    }
}
```

### next数组的思考题&#x20;

```java
 while (left > 0 && needle.charAt(left) != needle.charAt(right)) {
//        如果不相等就让left指针回退，到0时就停止回退
        left = next[left - 1];//进行回退操作；
}
```

这句代码干了什么？为什么要根据left = next\[left - 1]回退？直接回退到 0 位置可不可以？

<figure><img src="../../../.gitbook/assets/image (14).png" alt="" width="563"><figcaption></figcaption></figure>

可以看到，当 left 指针指到 f，right 指针指到 e的时候，已经出现 f 不等于 e的情况。

当前 left 指向的下标为 `5`，这时候，根据`left = next[left - 1]`的操作，`left` 指针并不会退回 0 的起点，而是根据**最长相同前后缀**的特点，让 `left` 退到 `ab`的位置。`left` 指针也就根据 `next[5-1]`所对应的 2 ，移动到了 下标为 2 的 `e` 的位置上。

<figure><img src="../../../.gitbook/assets/image (15).png" alt="" width="375"><figcaption></figcaption></figure>

接着将 `left` 指针指向的 `e` 和 `right` 指针指向的 `e` 进行对比，发现相等，就更新了当前数组的 **next 数组。**

![](https://pic.leetcode-cn.com/1641735890-bfTtHs-file\_1641735889847)
