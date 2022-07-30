### [112. 路径总和](https://leetcode.cn/problems/path-sum/)

- 简单

给你二叉树的根节点 `root` 和一个表示目标和的整数 `targetSum` 。判断该树中是否存在 **根节点到叶子节点**的路径，这条路径上所有节点值相加等于目标和 `targetSum` 。如果存在，返回 `true` ；否则，返回 `false` 。

**叶子节点** 是指没有子节点的节点。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2021/01/18/pathsum1.jpg)

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
输出：true
解释：等于目标和的根节点到叶节点路径如上图所示。
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg)

```
输入：root = [1,2,3], targetSum = 5
输出：false
解释：树中存在两条根节点到叶子节点的路径：
(1 --> 2): 和为 3
(1 --> 3): 和为 4
不存在 sum = 5 的根节点到叶子节点的路径。
```

**示例 3：**

```
输入：root = [], targetSum = 0
输出：false
解释：由于树是空的，所以不存在根节点到叶子节点的路径。
```

**解法一：BFS**

简单实现，BFS遍历时**将每层节点的值累加至下层节点**，同时**检查叶子节点的值是否等于`targetSum`**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root:
            return False
        queue = [root]
        while queue:
            tmp = []
            for node in queue:
                if not node.left and not node.right and node.val == targetSum:
                    return True
                if node.left:
                    node.left.val += node.val
                    tmp.append(node.left)
                if node.right:
                    node.right.val += node.val
                    tmp.append(node.right)
            queue = tmp
        return False
```

复杂实现：

用`now_sum`，`next_sum`数组记录当前层`q`对应的路径和以及下一层`tmp`对应的路径和

访问的话，为每层的节点加上索引属性`idx`，访问这层的路径和用`idx`，访问下层的用`node.left.val`或`node.right.val`，同时为下层节点`idx`赋值，用`len(tmp)`

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root:
            return False
        q = [(root, 0)]
        # 当前层的，到每一层，每个节点的tmp_sum
        now_sum = [root.val]
        while q:
            print(now_sum)
            # 检查叶子节点 + 和是否相等
            for i in range(len(now_sum)):
                if q[i][0].left == None and q[i][0].right == None and now_sum[i] == targetSum:
                    return True
            tmp = []
            next_sum = []
            for node, idx in q:
                if node.left:
                    tmp.append((node.left, len(tmp)))
                    next_sum.append(now_sum[idx]+node.left.val)
                if node.right:
                    tmp.append((node.right, len(tmp)))
                    next_sum.append(now_sum[idx]+node.right.val)
            q = tmp
            now_sum = next_sum
        return False
```

