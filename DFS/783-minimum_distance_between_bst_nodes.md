### [783. 二叉搜索树节点最小距离](https://leetcode.cn/problems/minimum-distance-between-bst-nodes/)

- 简单

给你一个二叉搜索树的根节点 `root` ，返回 **树中任意两不同节点值之间的最小差值** 。

差值是一个正数，其数值等于两值之差的绝对值。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2021/02/05/bst1.jpg)

```
输入：root = [4,2,6,1,3]
输出：1
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2021/02/05/bst2.jpg)

```
输入：root = [1,0,48,null,null,12,49]
输出：1
```

**提示：**

- 树中节点的数目范围是 `[2, 100]`
- `0 <= Node.val <= 10^5`

**解法一：迭代DFS**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minDiffInBST(self, root: TreeNode) -> int:
        ans, pre = float("inf"), -1
        stack = []
        while root or stack:      
            while root:
                stack.append(root)
                root = root.left
            root = stack.pop(-1)
            if pre == -1:
                pre = root.val
            else:
                ans = min(ans,root.val - pre)
                pre = root.val
            root = root.right 
        return ans
```

