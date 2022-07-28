### [515. 在每个树行中找最大值](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)

- 中等

给定一棵二叉树的根节点 `root` ，请找出该二叉树中每一层的最大值。

**示例1：**

 ![img](https://assets.leetcode.com/uploads/2020/08/21/largest_e1.jpg)

```
输入: root = [1,3,2,5,3,null,9]
输出: [1,3,9]
```

**示例2：**

```
输入: root = [1,2,3]
输出: [1,3]
```

**提示：**

- 二叉树的节点个数的范围是 `[0,10^4]`
- `-2^31 <= Node.val <= 2^31 - 1`

**解法一：队列**

队列，一次一层

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def largestValues(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        q = [root]
        ans = []
        while q:
            ans.append(max([node.val for node in q]))
            tmp = []
            for node in q:
                if node.left:
                    tmp.append(node.left)
                if node.right:
                    tmp.append(node.right)
            q = tmp
        return ans
```

**解法二：队列**

一次出队一个元素

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def largestValues(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        from collections import deque
        q = deque([root])
        ans = []
        while q:
            size = len(q)
            max_num = q[0].val
            # 遍历每一层
            for _ in range(size):
                node = q.popleft()
                max_num = node.val if node.val > max_num else max_num
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            ans.append(max_num)
        return ans
```

