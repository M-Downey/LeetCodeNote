### [100. 相同的树](https://leetcode.cn/problems/same-tree/)

- 简单

给你两棵二叉树的根节点 `p` 和 `q` ，编写一个函数来检验这两棵树是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

**示例 1：**

 ![image-20220404224151776](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220404224151776.png)

```
输入：p = [1,2,3], q = [1,2,3]
输出：true
```

**示例 2：**

 ![image-20220404224209420](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220404224209420.png)

```
输入：p = [1,2], q = [1,null,2]
输出：false
```

**示例 3：**

 ![image-20220404224225063](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220404224225063.png)

```
输入：p = [1,2,1], q = [1,1,2]
输出：false
```

**提示：**

- 两棵树上的节点数目都在范围 `[0, 100]` 内
- `-104 <= Node.val <= 104`

#### 解法一：递归地判断左右子树是否相同

思路：递归判断左右子树是否相同，递归终止条件有：**两结点为空，一结点为空，两结点值不同**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        if not p and not q:
            return True
        elif not p or not q:
            return False
        elif p.val != q.val:
            return False
        else:
            return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```

