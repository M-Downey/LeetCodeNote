### [958. 二叉树的完全性检验](https://leetcode.cn/problems/check-completeness-of-a-binary-tree/)

- 中等

给定一个二叉树的 `root` ，确定它是否是一个 *完全二叉树* 。

在一个 **[完全二叉树](https://baike.baidu.com/item/完全二叉树/7773232?fr=aladdin)** 中，除了最后一个关卡外，所有关卡都是完全被填满的，并且最后一个关卡中的所有节点都是尽可能靠左的。它可以包含 `1` 到 `2h` 节点之间的最后一级 `h` 。

**示例 1：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/complete-binary-tree-1.png)

```
输入：root = [1,2,3,4,5,6]
输出：true
解释：最后一层前的每一层都是满的（即，结点值为 {1} 和 {2,3} 的两层），且最后一层中的所有结点（{4,5,6}）都尽可能地向左。
```

**示例 2：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/15/complete-binary-tree-2.png)

```
输入：root = [1,2,3,4,5,null,7]
输出：false
解释：值为 7 的结点没有尽可能靠向左侧。
```

**提示：**

- 树的结点数在范围  `[1, 100]` 内。
- `1 <= Node.val <= 1000`

**解法一：BFS**

`BFS` 遍历，每个节点带上在本层中的序号，不是完全二叉树有2种情况：

- 除去最后一层，应该都是满的，所以比较每层应该有的长度和实际长度即可
- 最后一层节点应该都在左边，所以比较层长度和最后一个节点的序号即可

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isCompleteTree(self, root: Optional[TreeNode]) -> bool:
        # 每行依次编号，最后一个节点的编号应该是层长度减一
        # 之前的每一层要检查是否满了
        q = [(root, 0)]
        layer_sum = 1
        while q:
            tmp = []
            # print(q)
            for node, idx in q:
                if node.left:
                    tmp.append((node.left, 2 * idx))
                if node.right:
                    tmp.append((node.right, 2 * idx + 1))
            # q最后一层
            if not tmp:
                return len(q) - 1 == q[-1][1]
            # q倒数第二层
            if len(q) != layer_sum:
                return False
            layer_sum *= 2
            q = tmp
```

