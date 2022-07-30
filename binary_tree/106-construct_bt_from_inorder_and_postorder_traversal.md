### [106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

- 中等

给定两个整数数组 `inorder` 和 `postorder` ，其中 `inorder` 是二叉树的中序遍历，`postorder` 是同一棵树的后序遍历，请你构造并返回这颗 *二叉树* 。

**示例 1:**

 ![img](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
输入：inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
输出：[3,9,20,null,null,15,7]
```

**示例 2:**

```
输入：inorder = [-1], postorder = [-1]
输出：[-1]
```

**提示:**

- `1 <= inorder.length <= 3000`
- `postorder.length == inorder.length`

- `-3000 <= inorder[i], postorder[i] <= 3000`
- `inorder` 和 `postorder` 都由 **不同** 的值组成

- `postorder` 中每一个值都在 `inorder` 中
- `inorder` **保证**是树的中序遍历
- `postorder` **保证**是树的后序遍历

**解法一：递归**

中序：左中右

后序：左右中

从后序找`root`，分割中序的左子树右子树，再分割后序的左子树和右子树，再递归生成左子树和右子树

 来看一下一共分几步：

- 第一步：如果数组大小为零的话，说明是空节点了。
- 第二步：如果不为空，那么取后序数组最后一个元素作为节点元素。
- 第三步：找到后序数组最后一个元素在中序数组的位置，作为切割点
- 第四步：切割中序数组，切成中序左数组和中序右数组 （顺序别搞反了，一定是先切中序数组）
- 第五步：切割后序数组，切成后序左数组和后序右数组
- 第六步：递归处理左区间和右区间

![106.从中序与后序遍历序列构造二叉树](https://img-blog.csdnimg.cn/20210203154249860.png)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        if not inorder:
            return None
        rootValue = postorder[-1]
        root = TreeNode(rootValue)
        if len(postorder) == 1:
            return root
        delimiterIndex = 0
        while delimiterIndex < len(inorder):
            if inorder[delimiterIndex] == rootValue:
                break
            delimiterIndex += 1
        inorderLeft, inorderRight = inorder[:delimiterIndex], inorder[delimiterIndex+1:]
        postorderLeft, postorderRight = postorder[:len(inorderLeft)], postorder[len(inorderLeft):-1]    
        # print(inorderLeft, inorderRight)
        # print(postorderLeft, postorderRight)
        root.left = self.buildTree(inorderLeft, postorderLeft)
        root.right = self.buildTree(inorderRight, postorderRight)
        return root
```

