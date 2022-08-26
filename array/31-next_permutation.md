### [31. 下一个排列](https://leetcode.cn/problems/next-permutation/)

- 中等

整数数组的一个 **排列** 就是将其所有成员以序列或线性顺序排列。

- 例如，`arr = [1,2,3]` ，以下这些都可以视作 `arr` 的排列：`[1,2,3]`、`[1,3,2]`、`[3,1,2]`、`[2,3,1]` 。

整数数组的 **下一个排列** 是指其整数的下一个字典序更大的排列。更正式地，如果数组的所有排列根据其字典顺序从小到大排列在一个容器中，那么数组的 **下一个排列** 就是在这个有序容器中排在它后面的那个排列。如果不存在下一个更大的排列，那么这个数组必须重排为字典序最小的排列（即，其元素按升序排列）。

- 例如，`arr = [1,2,3]` 的下一个排列是 `[1,3,2]` 。

- 类似地，`arr = [2,3,1]` 的下一个排列是 `[3,1,2]` 。

- 而 `arr = [3,2,1]` 的下一个排列是 `[1,2,3]` ，因为 `[3,2,1]` 不存在一个字典序更大的排列。

给你一个整数数组 `nums` ，找出 `nums` 的下一个排列。

必须**[ 原地 ](https://baike.baidu.com/item/原地算法)**修改，只允许使用额外常数空间。

**示例 1：**

```
输入：nums = [1,2,3]
输出：[1,3,2]
```

**示例 2：**

```
输入：nums = [3,2,1]
输出：[1,2,3]
```

**示例 3：**

```
输入：nums = [1,1,5]
输出：[1,5,1]
```

**提示：**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 100`

**解法一：两边扫描+双指针**

找到尽量靠后的一个较小数，和右边的一个较大数，交换它们，并把较小数后面的数排成升序

从后向前找非降序，然后找到 `nums[i]` 再从后向前找第一个大于 `nums[i]` 的 `nums[j]`

1. 从后往前，非降序遍历，找到第一个变小的元素 `nums[i]`
2. 从后往前，找到第一个大于 `nums[i]` 的元素 `nums[j]`
3. 交换 `nums[i]` 和 `nums[j]` ，此时 `i + 1` 到 `n - 1` 是降序的，可以直接翻转

```python
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        # 找到尽量靠后的一个较小数，和右边的一个较大数，交换它们，并把较小数后面的数排成升序
        # 从后向前找非降序，然后找到 nums[i] 再从后向前找第一个大于 nums[i] 的 nums[j]
        n = len(nums)
        i = n - 2
        while i >= 0 and nums[i] >= nums[i + 1]:
            i -= 1
        if i >= 0:
            j = n - 1
            while j >= i + 1 and nums[j] <= nums[i]:
                j -= 1
            nums[i], nums[j] = nums[j], nums[i]

        left, right = i + 1, n - 1
        while left < right:
            nums[left], nums[right] = nums[right], nums[left]
            left += 1
            right -= 1
        return nums
```

