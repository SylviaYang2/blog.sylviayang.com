# Binary Tree

1. Binary Tree的第i层最多有2^(i-1)个节点；
2. 深度为k的Binary Tree至多有2^k - 1个节点；
3.

Full Binary Tree：深度为k，有2^k - 1个节点的树；除叶子节点外的所有节点都有两个子节点，节点数达到最大值；所有叶子节点在同一层上。

![](<../.gitbook/assets/image (64).png>)

Complete Binary Tree：若二叉树的深度为h，除第h层外，其他各层（1 ～ h-1）的节点数都达到最大个数，第h层所有的节点都连续集中在最左边

![](<../.gitbook/assets/image (96).png>)



### Binary Search Tree

1. All nodes of left subtree are less than the root node;
2. All nodes of right subtree are greater than the root node;
3. Both subtrees of each node are also BSTs, i.e., they all have the above two properties；
4. 用中序遍历（in-order traversal）可以得到有序数组；



### Operations: Search, Insert, Delete

1. **Search**：

```java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null) {
            return null;
        }
        if (root.val == val) {
            return root;
        } else if (root.val > val) {
            return searchBST(root.left, val);
        } else {
            return searchBST(root.right, val);
        }
    }
}
```

2. **Insert**：

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) {
            return new TreeNode(val);
        }

        if (val < root.val) {
            root.left = insertIntoBST(root.left, val);
        } else if (val > root.val) {
            root.right = insertIntoBST(root.right, val);
        }

        return root;
    }
}
```

3. **Delete**：

* 如果目标节点没有子节点，我们可以直接移除该目标节点。
* 如果目标节只有一个子节点，我们可以用其子节点作为替换。
* 如果目标节点有两个子节点，我们需要用其中序后继节点或者前驱节点来替换，再删除该目标节点。

<figure><img src="../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```java
两种方法：
       4
     /   \
    2     6
   / \   / \
  1   3 5   7
  
方法1:可能增加树的高度：
如果目标节点大于当前节点值，则去右子树中删除；
如果目标节点小于当前节点值，则去左子树中删除；
如果目标节点就是当前节点，分为以下三种情况：
其无左子：其右子顶替其位置，删除了该节点；
其无右子：其左子顶替其位置，删除了该节点；
其左右子节点都有：其左子树转移到其右子树的最左节点的左子树上，然后右子树顶替其位置，由此删除了该节点。

// 方法1，可能会增加树的高度
 public TreeNode deleteNode(TreeNode root, int key) {
     if (root == null) {
         return null;
     }
     // 如果key小于root，去左子树删除
     if (key < root.val) {
         root.left = deleteNode(root.left, key);
     } else if (key > root.val) { // 如果key大于root，去右子树删除
         root.right = deleteNode(root.right, key);
     } else { // 该node是要删除的key
         if (root.left == null) { // 该node无左子树
             return root.right;
         } else if (root.right == null) { // 该node无左子树
             return root.left;
         } else { // 左右子树都有: 把左子树挂到右子树的最左node下面
             TreeNode node = root.right;
             while (node.left != null) {
                 node = node.left;
             }
             node.left = root.left;
             return root.right;
         }
     }

     return root;
 }



方法2: 不会增加树的高度
如果目标节点没有子节点，我们可以直接移除该目标节点。
如果目标节只有一个子节点，我们可以用其子节点作为替换。
如果目标节点有两个子节点，我们需要用其中序后继节点（右子树的最小值）或者前驱节点（左子树的最大值）来替换（中序遍历BST返回的是有序数组），再删除该目标节点。

// 获取该node在有序数组里的下一个值
private int getNextValue(TreeNode node) {
    TreeNode right = node.right;
    while (right.left != null) {
        right = right.left;
    }
    return right.val;
}

// 获取该node在有序数组里的上一个值
private int getPreValue(TreeNode node) {
    TreeNode left = node.left;
    while (left.right != null) {
        left = left.right;
    }
    return left.val;
}


// 方法2：不会增加树的高度：
public TreeNode deleteNode(TreeNode root, int key) {
    if (root == null) {
        return null;
    }

    // 如果key小于root，去左子树删除
    if (key < root.val) {
        root.left = deleteNode(root.left, key);
    } else if (key > root.val) { // 如果key大于root，去右子树删除
        root.right = deleteNode(root.right, key);
    } else { // 该node是要删除的key
        if (root.left == null && root.right == null) { // 如果无左子树也无右子树，则直接删除当前节点
            root = null;
        } else if (root.right != null) {
            int nextVal = getNextValue(root);
            root.val = nextVal; // 用下一节点（右子树的最左节点）替换当前节点
            root.right = deleteNode(root.right, nextVal); // 去右子树上将替换的节点删除
        } else {
            int preVal = getPreValue(root);
            root.val = preVal; // 用上一节点（左子树的最右节点）替换当前节点
            root.left = deleteNode(root.left, preVal); // 去左子树上将替换的节点删除
        }
    }

    return root;
}

```
{% endcode %}

**Q: 为什么需要root.left = deleteNode(root.left, key) 来接收返回值，而不是直接执行没有返回值的deleteNode？**

**A：因为一般的删除操作，通过改变root下面连接的节点，达到删除的效果，而这里root.left = 想要返回的节点，如果返回的不是原左节点，则达到了删除root的效果。**



<figure><img src="../.gitbook/assets/image (115).png" alt=""><figcaption></figcaption></figure>

