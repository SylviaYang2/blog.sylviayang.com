# Binary Tree

1. Binary Tree的第i层最多有2^(i-1)个节点；
2. 深度为k的Binary Tree至多有2^k - 1个节点；
3.

Full Binary Tree：深度为k，有2^k - 1个节点的树；除叶子节点外的所有节点都有两个子节点，节点数达到最大值；所有叶子节点在同一层上。

![](../.gitbook/assets/image.png)

Complete Binary Tree：若二叉树的深度为h，除第h层外，其他各层（1 ～ h-1）的节点数都达到最大个数，第h层所有的节点都连续集中在最左边

![](<../.gitbook/assets/image (2).png>)



### Binary Search Tree

1. All nodes of left subtree are less than the root node;
2. All nodes of right subtree are greater than the root node;
3. Both subtrees of each node are also BSTs, i.e., they all have the above two properties；
4. 用中序遍历（in-order traversal）可以得到有序数组；



### Operations: Search, Insert, Delete

1. Search：

```
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

2. Insert：

```
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

1. Delete：
2.
3.
4. 如果目标节点没有子节点，我们可以直接移除该目标节点。
5. 如果目标节只有一个子节点，我们可以用其子节点作为替换。
6. 如果目标节点有两个子节点，我们需要用其中序后继节点或者前驱节点来替换，再删除该目标节点。

```
```

![](<../.gitbook/assets/image (3).png>)
