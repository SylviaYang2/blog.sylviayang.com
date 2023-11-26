# DFS

### Note:

1.  前序位置的代码只能从函数参数中获取父节点传递来的数据，而后序位置的代码不仅可以获取参数数据，还可以获取到子树通过函数返回值传递回来的数据。

    The code in the pre-order position can only obtain the data passed by the parent node from the function parameters, and the code in the post-order position can not only obtain the parameter data, but also the data passed back by the subtree through the function return value.

<figure><img src="../../.gitbook/assets/image (7) (1).png" alt="" width="375"><figcaption></figcaption></figure>

2. **前序遍历的代码在进入某一个节点之前的那个时间点执行，后序遍历代码在离开某个节点之后的那个时间点执行**。

### Pre-Order Traversal

**Recursive:**

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        if (root == null) {
            return res;
        }
        res.add(root.val);
        res.addAll(preorderTraversal(root.left));
        res.addAll(preorderTraversal(root.right));
        return res;
    }

    // Or
    
    public List<Integer> preorderTraversal(TreeNode root) { 
        List<Integer> res = new LinkedList<>(); 
        traverse(root, res); 
        return res; 
    }
    
    private void traverse(TreeNode root, List<Integer> res) {
        if (root == null) {
            return;
        }
        res.add(root.val);
        traverse(root.left);
        traverse(root.right);
    }
}
```

**Iterative:**

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<>();
        Deque<TreeNode> stack = new LinkedList<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            if (node != null) {
                res.add(node.val);
                stack.push(node.right);
                stack.push(node.left); 
            }
        }
        return res;
    }
}
```



### In-Order Traversal

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new LinkedList<>();
        traverse(root, result);
        return result;
    }
    
    private void traverse(TreeNode root, List<Integer> res) {
        if (root == null) {
            return;
        }
        traverse(root.left, res);
        res.add(root.val);
        traverse(root.right, res);
    }
}
```



### Post-Order Traversal

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new LinkedList<>();
        traverse(root, result);
        return result;
    }
    private void traverse(TreeNode root, List<Integer> res) {
        if (root == null) {
            return;
        }
        traverse(root.left, res);
        traverse(root.right, res);
        res.add(root.val);
    }
}
```
