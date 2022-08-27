### [257. 二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)

- 简单

给你一个二叉树的根节点 `root` ，按 **任意顺序** ，返回所有从根节点到叶子节点的路径。

**叶子节点** 是指没有子节点的节点。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg)

```
输入：root = [1,2,3,null,5]
输出：["1->2->5","1->3"]
```

**示例 2：**

```
输入：root = [1]
输出：["1"]
```

**提示：**

- 树中节点的数目在范围 `[1, 100]` 内
- `-100 <= Node.val <= 100`

**解法一：DFS**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        ans = []
        path = ''
        self.dfs(root, path, ans)
        # print(path)
        # print(ans)
        return ans
    
    def dfs(self, root, path, ans):
        if not root:
            return
        if root.left == None and root.right == None:
            ans.append(path + str(root.val))
            return
        path = path + str(root.val) + '->'
        self.dfs(root.left, path, ans)
        self.dfs(root.right, path, ans)
```

