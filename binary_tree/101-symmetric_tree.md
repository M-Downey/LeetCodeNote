### 101. 对称二叉树

给你一个二叉树的根节点 `root` ， 检查它是否轴对称。

**示例 1：**

 ![image-20220405231214599](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220405231214599.png)

```
输入：root = [1,2,2,3,4,4,3]
输出：true
```

**示例 2：**

 ![image-20220405231224933](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220405231224933.png)

```
输入：root = [1,2,2,null,3,null,3]
输出：false
```

**提示：**

- 树中节点数目在范围 `[1, 1000]` 内
- `-100 <= Node.val <= 100`

#### 解法一：递归判断

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        return self.check(root, root)
    def check(self, p: [TreeNode], q: [TreeNode]):
        if not p and not q:
            return True
        elif not p or not q:
            return False
        return p.val == q.val and self.check(p.left, q.right) and self.check(p.right, q.left)
```

