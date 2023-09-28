# Heap

### Definition

根据 维基百科 的定义，堆是一种特别的二叉树，满足以下条件的二叉树，可以称之为 堆：

1. 完全二叉树；&#x20;
2. 每一个节点的值都必须**大于等于**或者**小于等于**其孩子节点的值。&#x20;

堆具有以下的特点：

1. 可以在 O(logN) 的时间复杂度内向堆中插入元素；&#x20;
2. 可以在 O(logN) 的时间复杂度内向堆中删除元素；&#x20;
3. 可以在 O(1) 的时间复杂度内获取堆中的最大值或最小值。&#x20;
4. 在索引从0开始的数组中：
   * 父节点 i 的左子节点在位置（2 \* i + 1）
   * 父节点 i 的右子节点在位置（2 \* i + 2）
   * 子节点 i 的福节点在位置（floor (( i - 1 ) / 2）

堆的分类：最大堆和最小堆。

* 最大堆：堆中**每一个节点的值都大于等于**其孩子节点的值。所以最大堆的特性是堆顶元素（根节点）是堆中的最大值。
* 最小堆：堆中**每一个节点的值都小于等于**其孩子节点的值。所以最小堆的特性是堆顶元素（根节点）是堆中的最小值。



<figure><img src="../.gitbook/assets/image (119).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (91).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>
