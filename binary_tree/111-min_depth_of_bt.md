### [111. 二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

- 简单

给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

**说明：**叶子节点是指没有子节点的节点。

**示例 1：**

 ![image-20220412223657983](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220412223657983.png)

```
输入：root = [3,9,20,null,null,15,7]
输出：2
```

**示例 2：**

```
# 退化成链表
输入：root = [2,null,3,null,4,null,5,null,6]
输出：5
```

**提示：**

- 树中节点数的范围在 `[0, 105]` 内
- `-1000 <= Node.val <= 1000`

#### 解法1：类似层次遍历，用队列一次访问一层的结点

思路：类似层次遍历，用队列一次访问一层的结点，每次访问每层结点的儿子，如果左右孩子都是`null`，则可以返回`height`，否则将左右孩子添加入新的队列

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        queue = [root]
        height = 1
        while queue:
            tmp = []
            for node in queue:
                if not node.left and not node.right:
                    return height
                if node.left:
                    tmp.append(node.left)
                if node.right:
                    tmp.append(node.right)
            height += 1
            queue = tmp
```

