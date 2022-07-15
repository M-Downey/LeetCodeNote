### [107. 二叉树的层序遍历 II](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/)

- 中等

给你二叉树的根节点 `root` ，返回其节点值 **自底向上的层序遍历** 。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

**示例 1：**

 ![image-20220411003540344](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220411003540344.png)

```
输入：root = [3,9,20,null,null,15,7]
输出：[[15,7],[9,20],[3]]
```

**示例 2：**

```
输入：root = [1]
输出：[[1]]
```

**示例 3：**

```
输入：root = []
输出：[]
```

**提示：**

- 树中节点数目在范围 `[0, 2000]` 内
- `-1000 <= Node.val <= 1000`

#### 解法一：用队列，层次访问保存到结果中，最后翻转列表即可

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrderBottom(self, root: TreeNode) -> List[List[int]]:
        if not root:
            return []
        queue = [root]
        res = []
        while queue:
            res.append([node.val for node in queue])
            tmp = []
            for node in queue:
                if node.left:
                    tmp.append(node.left) 
                if node.right:
                    tmp.append(node.right)
            queue = tmp
        res.reverse()
        return res
```

