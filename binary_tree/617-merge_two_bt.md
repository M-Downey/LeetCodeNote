### [617. 合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/)

- 简单

给你两棵二叉树： `root1` 和 `root2` 。

想象一下，当你将其中一棵覆盖到另一棵之上时，两棵树上的一些节点将会重叠（而另一些不会）。你需要将这两棵树合并成一棵新二叉树。合并的规则是：如果两个节点重叠，那么将这两个节点的值相加作为合并后节点的新值；否则，**不为** null 的节点将直接作为新二叉树的节点。

返回合并后的二叉树。

**注意:** 合并过程必须从两个树的根节点开始。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2021/02/05/merge.jpg)

```
输入：root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
输出：[3,4,5,5,4,null,7]
```

**示例 2：**

```
输入：root1 = [1], root2 = [1,2]
输出：[2,2]
```

**提示：**

- 两棵树中的节点数目在范围 `[0, 2000]` 内
- `-10^4 <= Node.val <= 10^4`

**解法一：BFS**

在树1的基础上进行添加，队列中保存两者都有的节点

如果是两者都有节点，节点相加

如果是树1独有的，不用管

如果是树2独有的，需要把它赋值给树1

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root1:
            return root2
        if not root2:
            return root1
        from collections import deque
        # 队列中只放两树重合节点
        q = deque([root1, root2])
        while q:
            node1 = q.popleft()
            node2 = q.popleft()
            node1.val += node2.val
            if node1.left and node2.left:
                q.append(node1.left)
                q.append(node2.left)
            if node1.right and node2.right:
                q.append(node1.right)
                q.append(node2.right)
            if not node1.left and node2.left:
                node1.left = node2.left
            if not node1.right and node2.right:
                node1.right = node2.right
        return root1 
```

