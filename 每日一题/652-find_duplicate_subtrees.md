### [652. 寻找重复的子树](https://leetcode.cn/problems/find-duplicate-subtrees/)

- 中等

给定一棵二叉树 `root`，返回所有**重复的子树**。

对于同一类的重复子树，你只需要返回其中任意**一棵**的根结点即可。

如果两棵树具有**相同的结构**和**相同的结点值**，则它们是**重复**的。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2020/08/16/e1.jpg)

```
输入：root = [1,2,3,4,null,2,4,null,null,4]
输出：[[2,4],[4]]
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2020/08/16/e2.jpg)

```
输入：root = [2,1,1]
输出：[[1]]
```

**示例 3：**

 ![img](https://assets.leetcode.com/uploads/2020/08/16/e33.jpg)

```
输入：root = [2,2,2,3,null,3,null]
输出：[[2,3],[3]]
```

**提示：**

- 树中的结点数在`[1,10^4]`范围内。
- `-200 <= Node.val <= 200`

**解法一：DFS+树序列化+哈希表**

两棵树具有**相同的结构**和**相同的结点值**，则它们是**重复**的

本题关键是将重复的树转换为唯一的序列结构，然后建立序列结构和根节点的映射

记录所有树的序列结构，如果出现相同的序列结构即为重复的节点，添加到答案即可

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findDuplicateSubtrees(self, root: Optional[TreeNode]) -> List[Optional[TreeNode]]:
        def dfs(root):
            if not root:
                return ''
            string = str(root.val) + '(' + dfs(root.left) + ')' + '(' + dfs(root.right) + ')'
            if string in seen:
                ans.add(seen[string])
            else:
                seen[string] = root
            return string
        
        seen = dict()
        ans = set()
        dfs(root)
        return list(ans)
```

