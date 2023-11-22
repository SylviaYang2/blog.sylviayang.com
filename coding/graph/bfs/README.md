# BFS

### Level-Order Traversal

**Iterative:**

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

**Recursive:**

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

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt="" width="375"><figcaption></figcaption></figure>

### Visited:

Sometimes, it is important to make sure that we `never visit a node twice`. Otherwise, we might get stuck in an infinite loop, _e.g._ in graph with cycle. If so, we can add a hash set to the code above to solve this problem. Here is the pseudocode after modification:

```java
/**
 * Return the length of the shortest path between root and target node.
 */
int BFS(Node root, Node target) {
    Queue<Node> queue;  // store all nodes which are waiting to be processed
    Set<Node> visited;  // store all the nodes that we've visited
    int step = 0;       // number of steps neeeded from root to current node
    // initialize
    add root to queue;
    add root to visited;
    // BFS
    while (queue is not empty) {
        // iterate the nodes which are already in the queue
        int size = queue.size();
        for (int i = 0; i < size; ++i) {
            Node cur = the first node in queue;
            return step if cur is target;
            for (Node next : the neighbors of cur) {
                if (next is not in visited) {
                    add next to queue;
                    add next to visited;
                }
            }
            remove the first node from queue;
        }
        step = step + 1;
    }
    return -1;          // there is no path from root to target
}
```

###

### Reverse Level-Order Traversal

**Iterative:**

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

**Recursive:**

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
