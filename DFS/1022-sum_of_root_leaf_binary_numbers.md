### [1022. 从根到叶的二进制数之和](https://leetcode.cn/problems/sum-of-root-to-leaf-binary-numbers/)

- 简单

给出一棵二叉树，其上每个结点的值都是 `0` 或 `1` 。每一条从根到叶的路径都代表一个从最高有效位开始的二进制数。

- 例如，如果路径为 `0 -> 1 -> 1 -> 0 -> 1`，那么它表示二进制数 `01101`，也就是 `13` 。

对树上的每一片叶子，我们都要找出从根到该叶子的路径所表示的数字。

返回这些数字之和。题目数据保证答案是一个 **32 位** 整数。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2019/04/04/sum-of-root-to-leaf-binary-numbers.png)

```
输入：root = [1,0,1,0,1,0,1]
输出：22
解释：(100) + (101) + (110) + (111) = 4 + 5 + 6 + 7 = 22
```

**示例 2：**

```
输入：root = [0]
输出：0
```

**提示：**

- 树中的节点数在 `[1, 1000]` 范围内
- `Node.val` 仅为 `0` 或 `1` 

**解法一：DFS+回溯**

同 988 题

到每个叶子节点，计算一下，要回溯

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.path = []
        self.ans = 0

    def sumRootToLeaf(self, root: Optional[TreeNode]) -> int:
        # 和第 988 题一样
        self.dfs(root)
        return self.ans

    def dfs(self, root):
        if not root:
            return
        # 如果节点非空就加入 path
        self.path.append(root.val)
        if root.left == None and root.right == None:
            print(self.path)
            self.ans += self.trans(self.path)
            # 要么叶子节点直接返回(要pop)，要么注释掉下面两行，就在最后返回，反正叶子节点再调用子树是直接返回的
            self.path.pop()
            return
        
        self.dfs(root.left)
        self.dfs(root.right)
        self.path.pop()
    
    def trans(self, ls):
        ans = 0
        for num in ls:
            ans *= 2
            ans += num
        return ans
```

