# Binary Tree

1. BFS - Level Order Traversal

Solution 1:// 输入一棵二叉树的根节点，层序遍历这棵二叉树void levelTraverse(TreeNode root) {if (root == null) return;Queue\<TreeNode> q = new LinkedList<>();List\<List\<Integer>> wrapList = new LinkedList<>();q.offer(root);\
// 从上到下遍历二叉树的每一层while (!q.isEmpty()) {int sz = q.size();List\<Integer> subList = new LinkedList<>();// 从左到右遍历每一层的每个节点for (int i = 0; i < sz; i++) {TreeNode cur = q.poll();subList.add(cur.val);// 将下一层节点放入队列if (cur.left != null) {q.offer(cur.left);}if (cur.right != null) {q.offer(cur.right);\}}wrapList.add(subList);\}}\
![](https://en-cache/tokenKey%3D%22AuthToken%3AUser%3A231576898%22+a96b81ee-a63d-6750-a045-8da18106cdfc+ed3971ef100e9b4046b9daedbf7bef00+https://www.evernote.com/shard/s742/res/3aef06c7-6ea3-3103-f504-6f229230ccf5)Solution 2:public List\<List\<Integer>> levelOrderBottom(TreeNode root) {List\<List\<Integer>> res = new LinkedList<>();// Solution 2levelHelper(root, 0, res);return res;}private void levelHelper(TreeNode root, int depth, List\<List\<Integer>> res) {if (root == null) {return;}if (depth == res.size()) {res.add(new LinkedList\<Integer>());}res.get(depth).add(root.val);levelHelper(root.left, depth + 1, res);levelHelper(root.right, depth + 1, res);}\


2. Pre-Order Traversal

Solution 1:class Solution {public List\<Integer> preorderTraversal(TreeNode root) {List\<Integer> res = new LinkedList<>();if (root == null) {return res;}res.add(root.val);res.addAll(preorderTraversal(root.left));res.addAll(preorderTraversal(root.right));return res;}\
// Orpublic List\<Integer> preorderTraversal(TreeNode root) {List\<Integer> res = new LinkedList<>();traverse(root, res);return res;}private void traverse(TreeNode root, List\<Integer> res) {if (root == null) {return;}res.add(root.val);traverse(root.left);traverse(root.right);}\
Solution 2:class Solution {public List\<Integer> preorderTraversal(TreeNode root) {List\<Integer> res = new LinkedList<>();Deque\<TreeNode> stack = new LinkedList<>();stack.push(root);while (!stack.isEmpty()) {TreeNode node = stack.pop();if (node != null) {res.add(node.val);stack.push(node.right);stack.push(node.left);\}}return res;\}}

3. In-Order Traversal

class Solution {public List\<Integer> inorderTraversal(TreeNode root) {List\<Integer> result = new LinkedList<>();traverse(root, result);return result;}private void traverse(TreeNode root, List\<Integer> res) {if (root == null) {return;}traverse(root.left, res);res.add(root.val);traverse(root.right, res);\}}

4. Post-Order Traversal

class Solution {public List\<Integer> postorderTraversal(TreeNode root) {List\<Integer> result = new LinkedList<>();traverse(root, result);return result;}private void traverse(TreeNode root, List\<Integer> res) {if (root == null) {return;}traverse(root.left, res);traverse(root.right, res);res.add(root.val);\}}\


5. 前序位置的代码只能从函数参数中获取父节点传递来的数据，而后序位置的代码不仅可以获取参数数据，还可以获取到子树通过函数返回值传递回来的数据。

The code in the pre-order position can only obtain the data passed by the parent node from the function parameters, and the code in the post-order position can not only obtain the parameter data, but also the data passed back by the subtree through the function return value.\


6. Reverse level order traversal:

Solution 1:public List\<List\<Integer>> levelOrderBottom(TreeNode root) {List\<List\<Integer>> res = new LinkedList<>();Queue\<TreeNode> queue = new LinkedList<>();if (root == null) {return res;}queue.offer(root);while (!queue.isEmpty()) {int size = queue.size();List\<Integer> sub = new LinkedList<>();for (int i = 0; i < size; i++) {TreeNode cur = queue.poll();sub.add(cur.val);if (cur.left != null) {queue.offer(cur.left);}if (cur.right != null) {queue.offer(cur.right);\}}// add each level to the front of the result listres.add(0, sub);}return res;}Solution 2:public List\<List\<Integer>> levelOrderBottom(TreeNode root) {List\<List\<Integer>> res = new LinkedList<>();// Solution 2levelHelper(root, 0, res);Collections.reverse(res);return res;}private void levelHelper(TreeNode root, int depth, List\<List\<Integer>> res) {if (root == null) {return;}if (depth == res.size()) {res.add(new LinkedList\<Integer>());}res.get(depth).add(root.val);levelHelper(root.left, depth + 1, res);levelHelper(root.right, depth + 1, res);}\
