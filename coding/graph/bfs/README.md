# BFS

BFS 的核心思想应该不难理解的，就是把一些问题抽象成图，从一个点开始，向四周开始扩散。一般来说，我们写 BFS 算法都是用`「队列」`这种数据结构，每次将一个节点周围的所有节点加入队列。

BFS 相对 DFS 的最主要的区别是：**BFS 找到的路径一定是最短的，但代价就是空间复杂度可能比 DFS 大很多**，至于为什么，我们后面介绍了框架就很容易看出来了。

**1、为什么 BFS 可以找到最短距离，DFS 不行吗**？

首先，你看 BFS 的逻辑，`depth` 每增加一次，队列中的所有节点都向前迈一步，这保证了第一次到达终点的时候，走的步数是最少的。

DFS 不能找最短路径吗？其实也是可以的，但是时间复杂度相对高很多。你想啊，DFS 实际上是靠递归的堆栈记录走过的路径，你要找到最短路径，肯定得把二叉树中所有树杈都探索完才能对比出最短的路径有多长对不对？而 BFS 借助队列做到一次一步「齐头并进」，是可以在不遍历完整棵树的条件下找到最短距离的。

形象点说，DFS 是线，BFS 是面；DFS 是单打独斗，BFS 是集体行动。这个应该比较容易理解吧。

**2、既然 BFS 那么好，为啥 DFS 还要存在**？

BFS 可以找到最短距离，但是空间复杂度高，而 DFS 的空间复杂度较低。

还是拿刚才我们处理二叉树问题的例子，假设给你的这个二叉树是满二叉树，节点数为 `N`，对于 DFS 算法来说，空间复杂度无非就是递归堆栈，最坏情况下顶多就是树的高度，也就是 **`O(logN)`**。

但是你想想 BFS 算法，队列中每次都会储存着二叉树一层的节点，这样的话最坏情况下空间复杂度应该是树的最底层节点的数量，也就是 `N/2`，用 Big O 表示的话也就是 **`O(N)`**。

由此观之，BFS 还是有代价的，一般来说在找最短路径的时候使用 BFS，其他时候还是 DFS 使用得多一些（主要是递归代码好写）。

<figure><img src="../../../.gitbook/assets/image (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

### Level-Order Traversal

#### **Iterative:**

```java
// 输入一棵二叉树的根节点，层序遍历这棵二叉树
void levelTraverse(TreeNode root) {
    if (root == null) return;
    Queue<TreeNode> q = new LinkedList<>();
    List<List<Integer>> wrapList = new LinkedList<>();
    q.offer(root);

    // 从上到下遍历二叉树的每一层
    while (!q.isEmpty()) {
        int sz = q.size();
        List<Integer> subList = new LinkedList<>();
        // 从左到右遍历每一层的每个节点
        for (int i = 0; i < sz; i++) {
            TreeNode cur = q.poll();
            subList.add(cur.val);
            // 将下一层节点放入队列
            if (cur.left != null) {
                q.offer(cur.left);
            }
            if (cur.right != null) {
                q.offer(cur.right);
            }
        }
        wrapList.add(subList);
    }
}
```



### Visited:

Sometimes, it is important to make sure that we `never visit a node twice`. Otherwise, we might get stuck in an infinite loop, _e.g._ <mark style="color:red;">**in graph with cycle**</mark>. If so, we can add a hash set to the code above to solve this problem. Here is the pseudocode after modification:

```java
// 计算从起点 start 到终点 target 的最近距离
int BFS(Node start, Node target) {
    Queue<Node> q; // 核心数据结构
    Set<Node> visited; // 避免走回头路
    
    q.offer(start); // 将起点加入队列
    visited.add(start);

    while (q not empty) {
        int sz = q.size();
        /* 将当前队列中的所有节点向四周扩散 */
        for (int i = 0; i < sz; i++) {
            Node cur = q.poll();
            /* 划重点：这里判断是否到达终点 */
            if (cur is target)
                return step;
            /* 将 cur 的相邻节点加入队列 */
            for (Node x : cur.adj()) {
                if (x not in visited) {
                    q.offer(x);
                    visited.add(x);
                }
            }
        }
        step = step + 1;
    }
    // 如果走到这里，说明在图中没有找到目标节点
    return -1; // there is no path from root to target
}

```



#### **Recursive:**

```java
public List<List<Integer>> levelOrderBottom(TreeNode root) { 
        List<List<Integer>> res = new LinkedList<>(); 
        // Solution 2 
        levelHelper(root, 0, res); 
        return res; 
    } 
     
    private void levelHelper(TreeNode root, int depth, List<List<Integer>> res) { 
        if (root == null) { 
            return; 
        } 
        if (depth == res.size()) { 
            res.add(new LinkedList<Integer>()); 
        } 
        res.get(depth).add(root.val); 
        levelHelper(root.left, depth + 1, res); 
        levelHelper(root.right, depth + 1, res); 
    }
```





### 双向DFS：

<figure><img src="../../../.gitbook/assets/image (184).png" alt="" width="375"><figcaption></figcaption></figure>

图示中的树形结构，如果终点在最底部，按照传统 BFS 算法的策略，会把整棵树的节点都搜索一遍，最后找到 `target`；而双向 BFS 其实只遍历了半棵树就出现了交集，也就是找到了最短距离。从这个例子可以直观地感受到，双向 BFS 是要比传统 BFS 高效的。

**不过，双向 BFS 也有局限，因为你必须知道终点在哪里**。比如我们刚才讨论的二叉树最小高度的问题，你一开始根本就不知道终点在哪里，也就无法使用双向 BFS；但是第二个密码锁的问题，是可以使用双向 BFS 算法来提高效率的，代码稍加修改即可。

{% content-ref url="752.-open-the-lock-medium.md" %}
[752.-open-the-lock-medium.md](752.-open-the-lock-medium.md)
{% endcontent-ref %}





### Reverse Level-Order Traversal

#### **Iterative:**

```java
public List<List<Integer>> levelOrderBottom(TreeNode root) { 
        List<List<Integer>> res = new LinkedList<>(); 
        Queue<TreeNode> queue = new LinkedList<>(); 
        if (root == null) { 
            return res; 
        } 
        queue.offer(root); 
         
        while (!queue.isEmpty()) { 
            int size = queue.size(); 
            List<Integer> sub = new LinkedList<>(); 
             
            for (int i = 0; i < size; i++) { 
                TreeNode cur = queue.poll(); 
                sub.add(cur.val); 
                if (cur.left != null) { 
                    queue.offer(cur.left); 
                } 
                if (cur.right != null) { 
                    queue.offer(cur.right); 
                } 
            }
            // add each level to the front of the result list
            res.add(0, sub); 
        } 
        return res; 
}
```

#### **Recursive:**

```java
public List<List<Integer>> levelOrderBottom(TreeNode root) { 
        List<List<Integer>> res = new LinkedList<>(); 
        // Solution 2 
        levelHelper(root, 0, res); 
        Collections.reverse(res); // reverse
        return res; 
    } 
     
    private void levelHelper(TreeNode root, int depth, List<List<Integer>> res) { 
        if (root == null) { 
            return; 
        } 
        if (depth == res.size()) { 
            res.add(new LinkedList<Integer>()); 
        } 
        res.get(depth).add(root.val); 
        levelHelper(root.left, depth + 1, res); 
        levelHelper(root.right, depth + 1, res); 
    }
```
