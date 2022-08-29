### [988. 从叶结点开始的最小字符串](https://leetcode.cn/problems/smallest-string-starting-from-leaf/)

- 中等

给定一颗根结点为 `root` 的二叉树，树中的每一个结点都有一个 `[0, 25]` 范围内的值，分别代表字母 `'a'` 到 `'z'`。

返回 ***按字典序最小** 的字符串，该字符串从这棵树的一个叶结点开始，到根结点结束*。

注**：**字符串中任何较短的前缀在 **字典序上** 都是 **较小** 的：

- 例如，在字典序上 `"ab"` 比 `"aba"` 要小。叶结点是指没有子结点的结点。 

节点的叶节点是没有子节点的节点。

**示例 1：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/02/tree1.png)

```
输入：root = [0,1,2,3,4,3,4]
输出："dba"
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2019/01/30/tree2.png)

```
输入：root = [25,1,3,1,3,0,2]
输出："adz"
```

**示例 3：**

 ![img](https://assets.leetcode.com/uploads/2019/02/01/tree3.png)

```
输入：root = [2,2,1,null,1,0,null,0]
输出："abc"
```

**提示：**

- 给定树的结点数在 `[1, 8500]` 范围内
- `0 <= Node.val <= 25`

**解法一：DFS，回溯**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def __init__(self):
        self.indexs = []
        self.ans = '~' # 126

    def smallestFromLeaf(self, root: Optional[TreeNode]) -> str:
        from string import ascii_lowercase
        self.dfs(root)
        return self.ans

    def dfs(self, root):
        if not root:
            return
        # 添加节点
        self.indexs.append(root.val)
        if not root.left and not root.right:
            self.ans = min(''.join(reversed([ascii_lowercase[int(index)] for index in self.indexs])), self.ans)
        self.dfs(root.left)
        self.dfs(root.right)
        # 弹出节点
        self.indexs.pop()
```

