# Graph

{% embed url="https://labuladong.github.io/algo/di-yi-zhan-da78c/shou-ba-sh-03a72/tu-lun-ji--d55b2/" %}

<figure><img src="../../.gitbook/assets/image (43).png" alt="" width="375"><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```java
// 邻接表
// graph[x] 存储 x 的所有邻居节点
List<Integer>[] graph;

// 邻接矩阵
// matrix[x][y] 记录 x 是否有一条指向 y 的边
boolean[][] matrix;

对于邻接表，好处是占用的空间少。

但是，邻接表无法快速判断两个节点是否相邻。

比如说我想判断节点 1 是否和节点 3 相邻，我要去邻接表里 1 对应的邻居列表里查找 3 是否存在。但对于邻接矩阵就简单了，只要看看 matrix[1][3] 就知道了，效率高。

所以说，使用哪一种方式实现图，要看具体情况。
```
{% endcode %}

### Directed Weighted Graph

```java
// 邻接表
// graph[x] 存储 x 的所有邻居节点以及对应的权重
List<int[]>[] graph;

// 邻接矩阵
// matrix[x][y] 记录 x 指向 y 的边的权重，0 表示不相邻
int[][] matrix;

```

### Undirected Weighted Graph

<figure><img src="../../.gitbook/assets/image (45).png" alt="" width="563"><figcaption></figcaption></figure>

**在 `visited` 中被标记为 true 的节点用灰色表示，在 `onPath` 中被标记为 true 的节点用绿色表示**，类比贪吃蛇游戏，`visited` 记录蛇经过过的格子，而 `onPath` 仅仅记录蛇身。在图的遍历过程中，`onPath` 用于判断是否成环，类比当贪吃蛇自己咬到自己（成环）的场景，这下你可以理解它们二者的区别了吧。

```java
// 记录被遍历过的节点
boolean[] visited;
// 记录从起点到当前节点的路径
boolean[] onPath;

/* 图遍历框架 */
void traverse(Graph graph, int s) {
    if (visited[s]) return;
    // 经过节点 s，标记为已遍历
    visited[s] = true;
    // 做选择：标记节点 s 在路径上
    onPath[s] = true;
    for (int neighbor : graph.neighbors(s)) {
        traverse(graph, neighbor);
    }
    // 撤销选择：节点 s 离开路径
    onPath[s] = false;
}

```

这个 `onPath` 数组的操作很像前文 [回溯算法核心套路](https://labuladong.github.io/algo/di-ling-zh-bfe1b/hui-su-sua-c26da/) 中做「做选择」和「撤销选择」，区别在于位置：

* 回溯算法的「做选择」和「撤销选择」在 for 循环里面;
* 而DFS对 `onPath` 数组的操作在 for 循环外面。

```java
// DFS 算法，关注点在节点
void traverse(TreeNode root) {
    if (root == null) return;
    printf("进入节点 %s", root);
    for (TreeNode child : root.children) {
        traverse(child);
    }
    printf("离开节点 %s", root);
}

// 回溯算法，关注点在树枝
void backtrack(TreeNode root) {
    if (root == null) return;
    for (TreeNode child : root.children) {
        // 做选择
        printf("从 %s 到 %s", root, child);
        backtrack(child);
        // 撤销选择
        printf("从 %s 到 %s", child, root);
    }
}

```

如果执行这段代码，你会发现根节点被漏掉了：

```java
void traverse(TreeNode root) {
    if (root == null) return;
    for (TreeNode child : root.children) {
        printf("进入节点 %s", child);
        traverse(child);
        printf("离开节点 %s", child);
    }
}

```

所以对于这里「图」的遍历，我们应该用 DFS 算法，即把 `onPath` 的操作放到 for 循环外面，否则会漏掉记录起始点的遍历。
