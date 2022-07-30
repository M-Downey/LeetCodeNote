### [105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

- 中等

给定两个整数数组 `preorder` 和 `inorder` ，其中 `preorder` 是二叉树的**先序遍历**， `inorder` 是同一棵树的**中序遍历**，请构造二叉树并返回其根节点。

**示例 1:**

 ![img](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
输入: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
输出: [3,9,20,null,null,15,7]
```

**示例 2:**

```
输入: preorder = [-1], inorder = [-1]
输出: [-1]
```

**提示:**

- `1 <= preorder.length <= 3000`
- `inorder.length == preorder.length`

- `-3000 <= preorder[i], inorder[i] <= 3000`
- `preorder` 和 `inorder` 均 **无重复** 元素

- `inorder` 均出现在 `preorder`
- `preorder` **保证** 为二叉树的前序遍历序列
- `inorder` **保证** 为二叉树的中序遍历序列

**解法一：递归**

中序：左**中**右

前序：**中**左右

从前序找`root`，分割中序的左子树右子树，再分割前序的左子树和右子树，再递归生成左子树和右子树

 来看一下一共分几步：

- 第一步：如果数组大小为零的话，说明是空节点了。
- 第二步：如果不为空，那么取前序数组第一个元素作为节点元素。
- 第三步：找到前序数组第一个元素在中序数组的位置，作为切割点
- 第四步：切割中序数组，切成中序左数组和中序右数组 （顺序别搞反了，一定是先切中序数组）
- 第五步：切割前序数组，切成前序左数组和前序右数组
- 第六步：递归处理左区间和右区间

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        # 前：中左右
        # 中：左中右
        if not preorder:
            return None
        rootValue = preorder[0]
        root = TreeNode(rootValue)
        if len(preorder) == 1:
            return root
        delimiterIndex = 0
        while delimiterIndex < len(inorder):
            if inorder[delimiterIndex] == rootValue:
                break
            delimiterIndex += 1
        inorderLeft, inorderRight = inorder[:delimiterIndex], inorder[delimiterIndex+1:]
        preorderLeft, preorderRight = preorder[1:1+len(inorderLeft)], preorder[1+len(inorderLeft):]
        # print(preorderLeft, preorderRight)
        # print(inorderLeft, inorderRight)
        root.left = self.buildTree(preorderLeft, inorderLeft)
        root.right = self.buildTree(preorderRight, inorderRight)
        return root
```

