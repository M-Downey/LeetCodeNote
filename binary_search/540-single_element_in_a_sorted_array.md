### [540. 有序数组中的单一元素](https://leetcode.cn/problems/single-element-in-a-sorted-array/)

- 中等

给你一个仅由整数组成的有序数组，其中每个元素都会出现两次，唯有一个数只会出现一次。

请你找出并返回只出现一次的那个数。

你设计的解决方案必须满足 `O(log n)` 时间复杂度和 `O(1)` 空间复杂度。

**示例 1:**

```
输入: nums = [1,1,2,3,3,4,4,8,8]
输出: 2
```

**示例 2:**

```
输入: nums =  [3,3,7,7,10,11,11]
输出: 10
```

**提示:**

- `1 <= nums.length <= 10^5`
- `0 <= nums[i] <= 10^5`

**解法一：二分**

必须用 `[left, right)` 的二分，因为要检查的位置有 `nums[mid]` 和 `nums[mid+1]`(当 left 和 right 在边界重合时，mid+1会越界)

```python
class Solution:
    def singleNonDuplicate(self, nums: List[int]) -> int:
        # 有序数组，所以分割点 x 左边的 nums[y] == nums[y+1] 的y一定是偶数
        # 分割点 x 右边的 nums[z] == nums[z+1] 的z一定是奇数
        # 所以分割点 x 两边具有二段性
        # 如果 mid 是偶数且 nums[mid] == nums[mid+1]
        # 或 mid 是奇数且 nums[mid-1] == nums[mid]
        # 就说明 mid 处于x左边，区间要右移
        low, high = 0, len(nums) - 1
        while low < high:
            mid = (low + high) // 2
            if nums[mid] == nums[mid ^ 1]:
                low = mid + 1
            else:
                high = mid
        return nums[low]
```

