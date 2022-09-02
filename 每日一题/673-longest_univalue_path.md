### [687. 最长同值路径](https://leetcode.cn/problems/longest-univalue-path/)

- 中等

给定一个二叉树的 `root` ，返回 *最长的路径的长度* ，这个路径中的 *每个节点具有相同值* 。 这条路径可以经过也可以不经过根节点。

**两个节点之间的路径长度** 由它们之间的边数表示。

**示例 1:**

 ![img](https://assets.leetcode.com/uploads/2020/10/13/ex1.jpg)

```
输入：root = [5,4,5,1,1,5]
输出：2
```

**示例 2:**

 ![img](https://assets.leetcode.com/uploads/2020/10/13/ex2.jpg)

```
输入：root = [1,4,5,4,4,5]
输出：2
```

**提示:**

- 树的节点数的范围是 `[0, 10^4]` 
- `-1000 <= Node.val <= 1000`
- 树的深度将不超过 `1000` 

**解法一：DFS**

`DFS` 求左右子树中较大的那个同值路径长度

对于当前节点 `root` 来说，求出左右子树的最长同值路径后，再和 `root.val` 比较，如果相等的话，就把它们加一，并且记录这两值之和，求这个值的最大值

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.ans = 0

    def longestUnivaluePath(self, root: Optional[TreeNode]) -> int:
        self.dfs(root)
        return self.ans
    
    # 返回某节点的左右子树中的更长的同值路径长度
    def dfs(self, root):
        if not root:
            return 0
        left = self.dfs(root.left)
        right = self.dfs(root.right)
        if root.left and root.left.val == root.val:
            left1 = left + 1
        else:
            left1 = 0
        if root.right and root.right.val == root.val:
            right1 = right + 1
        else:
            right1 = 0
        self.ans = max(self.ans, left1 + right1)
        return max(left1, right1)
```

