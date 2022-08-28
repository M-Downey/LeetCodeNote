### [671. 二叉树中第二小的节点](https://leetcode.cn/problems/second-minimum-node-in-a-binary-tree/)

- 简单

给定一个非空特殊的二叉树，每个节点都是正数，并且每个节点的子节点数量只能为 `2` 或 `0`。如果一个节点有两个子节点的话，那么该节点的值等于两个子节点中较小的一个。

更正式地说，即 `root.val = min(root.left.val, root.right.val)` 总成立。

给出这样的一个二叉树，你需要输出所有节点中的 **第二小的值** 。

如果第二小的值不存在的话，输出 -1 **。**

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2020/10/15/smbt1.jpg)

```
输入：root = [2,2,5,null,null,5,7]
输出：5
解释：最小的值是 2 ，第二小的值是 5 。
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2020/10/15/smbt2.jpg)

```
输入：root = [2,2,2]
输出：-1
解释：最小的值是 2, 但是不存在第二小的值。
```

**提示：**

- 树中节点数目在范围 `[1, 25]` 内
- `1 <= Node.val <= 2^31 - 1`

- 对于树中每个节点 `root.val == min(root.left.val, root.right.val)`

**解法一：DFS**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.ans = float('inf')

    def findSecondMinimumValue(self, root: Optional[TreeNode]) -> int:
        # 有全都相等的情况，所以不是第二层中较大的数
        # 根节点是最小的数，要求比根节点大的最小数
        value = root.val
        self.dfs(root, value)
        return self.ans if self.ans != float('inf') else -1

    def dfs(self, root, value):
        if not root:
            return
        if root.val > value and root.val < self.ans:
            self.ans = root.val
        if root.val > value:
            return
        self.dfs(root.left, value)
        self.dfs(root.right, value)
```

