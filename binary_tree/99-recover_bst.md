### [99. 恢复二叉搜索树](https://leetcode.cn/problems/recover-binary-search-tree/)

- 中等

给你二叉搜索树的根节点 `root` ，该树中的 **恰好** 两个节点的值被错误地交换。*请在不改变其结构的情况下，恢复这棵树* 。

**示例 1：**

 ![image-20220405231504993](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220405231504993.png)

```
输入：root = [1,3,null,null,2]
输出：[3,1,null,null,2]
解释：3 不能是 1 的左孩子，因为 3 > 1 。交换 1 和 3 使二叉搜索树有效。
```

**示例 2：**

 ![image-20220405231615041](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220405231615041.png)

```
输入：root = [3,1,4,null,null,2]
输出：[2,1,4,null,null,3]
解释：2 不能在 3 的右子树中，因为 2 < 3 。交换 2 和 3 使二叉搜索树有效。
```

**提示：**

- 树上节点的数目在范围 `[2, 1000]` 内
- `-231 <= Node.val <= 231 - 1`

#### 解法一：中序遍历找到应该交换的结点，再递归交换结点

思路：中序遍历本应是递增的序列，但是交换后（设为i，j），如果两元素非相邻，则`a[i] > a[i+1]`且`a[j-1]>a[j]`，可以找到i和j，然后返回两值；如果相邻，则找到相等的i和j，返回`a[i]`和`a[i+1]`，最后用递归交换两结点的值。

注意点：

1. 递归终点是：交换的次数为2
2. 交换的条件是：结点的值是两值中的一个。然后将结点赋值为另一值。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def recoverTree(self, root: Optional[TreeNode]) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        in_order = []
        self.inOrder(root, in_order)
        print(f'In order result: {in_order}')
        swapped = self.findSwapped(in_order)
        print(f'Element to be swapped: {swapped}')
        self.recover(root, 2, swapped[0], swapped[1])
        print(root)

    def inOrder(self, root, nums):
        if not root:
            return
        self.inOrder(root.left, nums)
        nums.append(root.val)
        self.inOrder(root.right, nums)
    def findSwapped(self, nums):
        idx = []
        for i in range(len(nums) - 1):
            if nums[i] > nums[i + 1]:
                idx.append(i)
        if len(idx) == 1:
            x = nums[idx[0]]
            y = nums[idx[0] + 1]
        else:
            x = nums[idx[0]]
            y = nums[idx[1] + 1]
        return [x, y]
    def recover(self, root, count, x, y):
        if root:
            if root.val == x or root.val == y:
                root.val = y if root.val == x else x
                count -= 1
                if count == 0:
                    return
            self.recover(root.right, count, x, y)
            self.recover(root.left, count, x, y)
```

