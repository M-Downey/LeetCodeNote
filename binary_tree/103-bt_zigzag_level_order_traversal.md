### 103. 二叉树的锯齿形层序遍历

给你二叉树的根节点 `root` ，返回其节点值的 **锯齿形层序遍历** 。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

**示例 1：**

 ![image-20220406225840065](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220406225840065.png)

```
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[20,9],[15,7]]
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
- `-100 <= Node.val <= 100`

#### 解法一：队列每次访问一层，用flag标志偶数层要翻转列表

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
        if not root:
            return []
        q = [root]
        res = []
        flag = 1
        while q:
            tmp = [node.val for node in q]
            if flag % 2 == 0:
                tmp.reverse()
            res.append(tmp)
            next_q = []
            for node in q:
                if node.left:
                    next_q.append(node.left)
                if node.right:
                    next_q.append(node.right)
            q = next_q
            flag += 1
        print(res)
        return res
```

