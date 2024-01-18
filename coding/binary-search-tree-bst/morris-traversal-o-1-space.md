# Morris Traversal - O(1) Space

### Pre-Order Traversal

新建临时节点，令该节点为 root；

1. 如果当前节点的左子节点为空，将当前节点加入答案，并遍历当前节点的右子节点；
2. 如果当前节点的左子节点不为空，在当前节点的左子树中找到当前节点在中序遍历下的`predecessor`：

* 如果`predecessor`的右子节点为空，将`predecessor`的右子节点设置为当前节点（create cycle）。然后将当前节点加入答案，并将`predecessor`的右子节点更新为当前节点。当前节点更新为当前节点的左子节点。
* 如果`predecessor`的右子节点为当前节点，将它的右子节点重新设为null。当前节点更新为当前节点的右子节点。

3. 重复步骤 2 和步骤 3，直到遍历结束。

其实整个过程我们就多做一步：假设当前遍历到的节点为 x，将 x 的左子树中最右边的节点的右孩子指向 x，这样在左子树遍历完成后我们通过这个指向**走回了 x**，且能通过这个指向知晓我们已经遍历完成了左子树，而不用再通过栈来维护，省去了栈的空间复杂度。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        TreeNode predecessor = null;

        while (root != null) {
            if (root.left != null) {
                // predecessor 节点就是当前 root 节点向左走一步，然后一直向右走至无法走为止
                predecessor = root.left;
                while (predecessor.right != null && predecessor.right != root) {
                    predecessor = predecessor.right;
                }

                // 让 predecessor 的右指针指向 root，继续遍历左子树
                if (predecessor.right == null) {
                    res.add(root.val);
                    predecessor.right = root;
                    root = root.left;
                }
                // 说明左子树已经访问完了，我们需要断开链接
                else {
                    predecessor.right = null;
                    root = root.right;
                }
            } else { // 如果没有左孩子，则直接访问右孩子
                res.add(root.val);
                root = root.right;
            }
        }
        return res;
    }
}
```



### In-Order Traversal

<figure><img src="../../.gitbook/assets/image (7).png" alt="" width="563"><figcaption></figcaption></figure>

```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> res = new ArrayList<Integer>();
    TreeNode predecessor = null;

    while (root != null) {
        if (root.left != null) {
            // predecessor 节点就是当前 root 节点向左走一步，然后一直向右走至无法走为止
            predecessor = root.left;
            while (predecessor.right != null && predecessor.right != root) {
                predecessor = predecessor.right;
            }

            // 让 predecessor 的右指针指向 root，继续遍历左子树
            if (predecessor.right == null) {
                predecessor.right = root;
                root = root.left;
            }
            // 说明左子树已经访问完了，我们需要断开链接
            else {
                res.add(root.val);
                predecessor.right = null;
                root = root.right;
            }
        } else { // 如果没有左孩子，则直接访问右孩子
            res.add(root.val);
            root = root.right;
        }
    }

    return res;
}
```



### Post-Order Traversal

和前面一样，唯一不同的是：倒序输出从当前节点的左子节点到该前驱节点这条路径上的所有节点。

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if (root == null) {
            return res;
        }
        TreeNode predecessor = null;

        while (root != null) {
            predecessor = root.left;
            if (predecessor != null) {
                while (predecessor.right != null && predecessor.right != root) {
                    predecessor = predecessor.right;
                }
                if (predecessor.right == null) {
                    predecessor.right = root;
                    root = root.left;
                    continue;
                } else {
                    predecessor.right = null;
                    addPath(res, root.left);
                }
            }
            root = root.right;
        }
        addPath(res, root);
        return res;
    }

    public void addPath(List<Integer> res, TreeNode node) {
        int count = 0;
        while (node != null) {
            ++count;
            res.add(node.val);
            node = node.right;
        }
        int left = res.size() - count, right = res.size() - 1;
        while (left < right) {
            int temp = res.get(left);
            res.set(left, res.get(right));
            res.set(right, temp);
            left++;
            right--;
        }
    }
}
```
