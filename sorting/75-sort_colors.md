### [75. 颜色分类](https://leetcode.cn/problems/sort-colors/)

- 中等

给定一个包含红色、白色和蓝色、共 `n` 个元素的数组 `nums` ，**[原地](https://baike.baidu.com/item/原地算法)**对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

我们使用整数 `0`、 `1` 和 `2` 分别表示红色、白色和蓝色。

必须在不使用库的sort函数的情况下解决这个问题。

 **示例 1：**

```
输入：nums = [2,0,2,1,1,0]
输出：[0,0,1,1,2,2]
```

**示例 2：**

```
输入：nums = [2,0,1]
输出：[0,1,2]
```

**提示：**

- `n == nums.length`
- `1 <= n <= 300`
- `nums[i]` 为 `0`、`1` 或 `2`

**进阶：**

- 你可以不使用代码库中的排序函数来解决这道题吗？
- 你能想出一个仅使用常数空间的一趟扫描算法吗？

**解法一：单指针**

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        # 单指针扫两遍，单指针 p 指向要填入的位置，然后遍历 nums 进行交换
        p = 0
        n = len(nums)
        for i in range(n):
            if nums[i] == 0:
                nums[p], nums[i] = nums[i], nums[p]
                p += 1
        for i in range(n):
            if nums[i] == 1:
                nums[p], nums[i] = nums[i], nums[p]
                p += 1
        return nums
```

**解法二：双指针**

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        # 双指针扫一遍，p0指向要放0的位置，p1指向要放1的位置
        # 如果已经存放了 1 了，那么交换nums[p0]和nums[i]会把1交换过去，所以要再换一遍nums[p1]和nums[i]
        n = len(nums)
        p0, p1 = 0, 0
        for i in range(n):
            if nums[i] == 1:
                nums[p1], nums[i] = nums[i], nums[p1]
                p1 += 1
            if nums[i] == 0:
                nums[p0], nums[i] = nums[i], nums[p0]
                if p0 < p1:
                    nums[p1], nums[i] = nums[i], nums[p1]
                p0 += 1
                p1 += 1
        return nums
```

