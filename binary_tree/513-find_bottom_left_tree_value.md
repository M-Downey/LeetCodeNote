### [513. 找树左下角的值](https://leetcode.cn/problems/find-bottom-left-tree-value/)

- 中等

给定一个二叉树的 **根节点** `root`，请找出该二叉树的 **最底层 最左边** 节点的值。

假设二叉树中至少有一个节点。

**示例 1:**

 ![img](https://assets.leetcode.com/uploads/2020/12/14/tree1.jpg)

```
输入: root = [2,1,3]
输出: 1
```

**示例 2:**

 ![img](https://assets.leetcode.com/uploads/2020/12/14/tree2.jpg)

```
输入: [1,2,3,4,null,5,6,null,null,7]
输出: 7
```

**提示:**

- 二叉树的节点个数的范围是 `[1,10^4]`
- `-2^31 <= Node.val <= 2^31 - 1` 

**解法一：BFS**

遍历到最后一层（即下一层节点是0个）时，返回此时队列中第一个节点的值即可

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        q = [root]
        ans = 0
        while q:
            tmp = []
            for node in q:
                if node.left:
                    tmp.append(node.left)
                if node.right:
                    tmp.append(node.right)
            if not tmp:
                ans = q[0].val
                break
            q = tmp
        return ans
```

