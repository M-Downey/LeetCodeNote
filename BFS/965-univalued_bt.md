### [965. 单值二叉树](https://leetcode.cn/problems/univalued-binary-tree/)

- 简单

如果二叉树每个节点都具有相同的值，那么该二叉树就是*单值*二叉树。

只有给定的树是单值二叉树时，才返回 `true`；否则返回 `false`。

**示例 1：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/29/screen-shot-2018-12-25-at-50104-pm.png)

```
输入：[1,1,1,1,1,null,1]
输出：true
```

**示例 2：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/29/screen-shot-2018-12-25-at-50050-pm.png)

```
输入：[2,2,2,5,2]
输出：false
```

**提示：**

1. 给定树的节点数范围是 `[1, 100]`。
2. 每个节点的值都是整数，范围为 `[0, 99]` 。

**解法一：BFS**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isUnivalTree(self, root: Optional[TreeNode]) -> bool:
        q = [root]
        while q:
            tmp = []
            for node in q:
                if node.val != root.val:
                    return False
                if node.left:
                    tmp.append(node.left)
                if node.right:
                    tmp.append(node.right)
            q = tmp
        return True
```

