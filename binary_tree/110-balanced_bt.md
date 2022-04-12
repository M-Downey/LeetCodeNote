### 110. 平衡二叉树

给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

> 一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过 1 。

**示例 1：**

 ![image-20220412224027579](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220412224027579.png)

```
输入：root = [3,9,20,null,null,15,7]
输出：true
```

**示例 2：**

 ![image-20220412224046553](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220412224046553.png)

```
输入：root = [1,2,2,3,3,null,null,4,4]
输出：false
```

**示例 3：**

```
输入：root = []
输出：true
```

**提示：**

- 树中的节点数在范围 `[0, 5000]` 内
- `-104 <= Node.val <= 104`

#### 解法一：递归

思路：借助求深度的函数，递归地判断左右子树是否是平衡二叉树

该树是平衡二叉树的条件有三：

1. 左子树是平衡二叉树
2. 右子树是平衡二叉树
3. 左右子树高度差<=1

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        return abs(self.getDepth(root.left) - self.getDepth(root.right)) < 1 and isBalanced(root.left_h) and isBalanced(root.right)
    def getDepth(self, root):
        if not root:
            return 0
        left_h = self.getDepth(root.left)
        right_h = self.getDepth(root.right)
        h = left_h if left_h > right_h else right_h
        return h + 1
```

