# BackTracking (节点 VS 树枝)

{% embed url="https://labuladong.github.io/algo/di-ling-zh-bfe1b/hui-su-sua-c26da/" %}

站在回溯树的一个节点上，你只需要思考 3 个问题：

1、路径：也就是已经做出的选择。

2、选择列表：也就是你当前可以做的选择。

3、结束条件：也就是到达决策树底层，无法再做选择的条件。

```python
result = []
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return
    
    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择

```

{% content-ref url="array/backtracking/46.-permutations-medium.md" %}
[46.-permutations-medium.md](array/backtracking/46.-permutations-medium.md)
{% endcontent-ref %}







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
