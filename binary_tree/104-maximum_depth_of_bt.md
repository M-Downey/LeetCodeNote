### [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

- 简单

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**说明:** 叶子节点是指没有子节点的节点。

**示例：**

给定二叉树 `[3,9,20,null,null,15,7]`，

```
    3
   / \
  9  20
    /  \
   15   7
```

返回它的最大深度 3 。

#### 解法一：递归

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        return self.max(self.maxDepth(root.left), self.maxDepth(root.right))  + 1
    def max(self, a, b):
        return a if a > b else b
```

**解法二：BFS**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        cnt = 0
        if not root:
            return cnt
        q = [root]
        while q:
            cnt += 1
            tmp = []
            for node in q:
                ln = node.left
                rn = node.right
                if ln:
                    tmp.append(ln)
                if rn:
                    tmp.append(rn)
            q = tmp 
        return cnt
```

