### [1161. 最大层内元素和](https://leetcode.cn/problems/maximum-level-sum-of-a-binary-tree/)

- 中等

给你一个二叉树的根节点 `root`。设根节点位于二叉树的第 `1` 层，而根节点的子节点位于第 `2` 层，依此类推。

请返回层内元素之和 **最大** 的那几层（可能只有一层）的层号，并返回其中 **最小** 的那个。

**示例 1：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/08/17/capture.jpeg)

```
输入：root = [1,7,0,7,-8,null,null]
输出：2
解释：
第 1 层各元素之和为 1，
第 2 层各元素之和为 7 + 0 = 7，
第 3 层各元素之和为 7 + -8 = -1，
所以我们返回第 2 层的层号，它的层内元素之和最大。
```

**示例 2：**

```
输入：root = [989,null,10250,98693,-89388,null,null,null,-32127]
输出：2
```

**提示：**

- 树中的节点数在 `[1, 10^4]`范围内
- `-10^5 <= Node.val <= 10^5`

**解法一：BFS**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxLevelSum(self, root: Optional[TreeNode]) -> int:
        maxSum = float('-inf')
        ans = 1
        q = [(root, 1)]
        while q:
            tmp = []
            nowSum = sum([node.val for node, _ in q])
            layer = q[-1][1]
            if nowSum > maxSum:
                maxSum = nowSum
                ans = layer
            for node, layer in q:
                if node.left:
                    tmp.append((node.left, layer + 1))
                if node.right:
                    tmp.append((node.right, layer + 1))
            q = tmp
        return ans
```

**解法二：DFS**

`DFS` 遍历树带上当前深度，维护一个动态数组用于保存每个深度的节点和，最后返回最大的即可

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxLevelSum(self, root: Optional[TreeNode]) -> int:
        level_sum = []
        # root 一定非空，所以直接判断左右子树就可
        def dfs(root, level):
            if level == len(level_sum):
                level_sum.append(root.val)
            else:
                level_sum[level] += root.val
            if root.left:
                dfs(root.left, level + 1)
            if root.right:
                dfs(root.right, level + 1)
        dfs(root, 0)
        return level_sum.index(max(level_sum)) + 1
```

