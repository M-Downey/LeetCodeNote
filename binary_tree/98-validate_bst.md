### [98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)

- 中等

给你一个二叉树的根节点 `root` ，判断其是否是一个有效的二叉搜索树。

**有效** 二叉搜索树定义如下：

- 节点的左子树只包含 **小于** 当前节点的数。
- 节点的右子树只包含 **大于** 当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

**示例 1：**

 ![image-20220404223014186](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220404223014186.png)

```
输入：root = [2,1,3]
输出：true
```

**示例 2：**

 ![image-20220404223039558](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220404223039558.png)

```
输入：root = [5,1,4,null,null,3,6]
输出：false
解释：根节点的值是 5 ，但是右子节点的值是 4 。
```

**提示：**

- 树中节点数目范围在`[1, 104]` 内
- `-231 <= Node.val <= 231 - 1`

#### 解法一：用中序遍历，如果是升序就是（出现<=就错误）

思路：用`pre`保存上一个结点的值，初始化为`-inf`

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        stack = []
        pre = float('-inf')
        while stack or root:
            while root:
                stack.append(root)
                root = root.left
            root = stack.pop()
            if root.val <= pre:
                return False
            pre = root.val
            root = root.right
        return True
```

