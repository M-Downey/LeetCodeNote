### 108. 将有序数组转换为二叉搜索树

给你一个整数数组 `nums` ，其中元素已经按 **升序** 排列，请你将其转换为一棵 **高度平衡** 二叉搜索树。

**高度平衡** 二叉树是一棵满足「每个节点的左右两个子树的高度差的绝对值不超过 1 」的二叉树。

**示例 1：**

 ![image-20220411232445656](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220411232445656.png)

```
输入：nums = [-10,-3,0,5,9]
输出：[0,-3,9,-10,null,5]
解释：[0,-10,5,null,-3,null,9] 也将被视为正确答案：
```

 ![image-20220411232527889](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220411232527889.png)

**示例 2：**

 ![image-20220411232547642](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220411232547642.png)

```
输入：nums = [1,3]
输出：[3,1]
解释：[1,null,3] 和 [3,1] 都是高度平衡二叉搜索树。
```

**提示：**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` 按 **严格递增** 顺序排列

#### 解法一：递归，每次选择数组中间（偶数就靠左）的元素作为根结点

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        return self.helper(0, len(nums) - 1, nums)
    def helper(self, left, right, nums):
        if left > right:
            return None
        mid = (left + right) // 2 
        root = TreeNode(nums[mid])
        root.left = self.helper(left, mid - 1, nums)
        root.right = self.helper(mid + 1, right, nums)
        return root
```
