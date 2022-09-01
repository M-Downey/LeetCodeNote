### [653. 两数之和 IV - 输入二叉搜索树](https://leetcode.cn/problems/two-sum-iv-input-is-a-bst/)

- 简单

给定一个二叉搜索树 `root` 和一个目标结果 `k`，如果二叉搜索树中存在两个元素且它们的和等于给定的目标结果，则返回 `true`。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2020/09/21/sum_tree_1.jpg)

```
输入: root = [5,3,6,2,4,null,7], k = 9
输出: true
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2020/09/21/sum_tree_2.jpg)

```
输入: root = [5,3,6,2,4,null,7], k = 28
输出: false
```

**提示:**

- 二叉树的节点个数的范围是 `[1, 104]`.
- `-10^4 <= Node.val <= 10^4`

- 题目数据保证，输入的 `root` 是一棵 **有效** 的二叉搜索树
- `-10^5 <= k <= 10^5`

**解法一：中序遍历**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findTarget(self, root: Optional[TreeNode], k: int) -> bool:
        inOrder = []
        self.dfs(root, inOrder)
        n = len(inOrder)
        uset = set()
        for i in range(n):
            if k - inOrder[i] in uset:
                return True
            uset.add(inOrder[i])
        return False

    def dfs(self, root, inOrder):
        if not root:
            return
        self.dfs(root.left, inOrder)
        inOrder.append(root.val)
        self.dfs(root.right, inOrder)
```

