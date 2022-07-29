### [404. 左叶子之和](https://leetcode.cn/problems/sum-of-left-leaves/)

- 简单

给定二叉树的根节点 `root` ，返回所有左叶子之和。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2021/04/08/leftsum-tree.jpg)

```
输入: root = [3,9,20,null,null,15,7] 
输出: 24 
解释: 在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24
```

**示例 2：**

```
输入: root = [1]
输出: 0
```

**提示:**

- 节点数在 `[1, 1000]` 范围内
- `-1000 <= Node.val <= 1000`

**解法一：BFS**

给每个节点打上标记，左节点是1，右节点是0，如果是左节点而且是叶子节点的话，就加上这个节点的值

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        if not root.left and not root.right:
            return 0
        q = [(root, 0)]
        ans = 0
        while q:
            tmp = []
            for node, flag in q:
                if node.left:
                    tmp.append((node.left, 1))
                if node.right:
                    tmp.append((node.right, 0))
                if node.left == None and node.right == None and flag == 1:
                    ans += node.val
            q = tmp

        return ans
```

