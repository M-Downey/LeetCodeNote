### [977. 有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

- 简单

给你一个按 **非递减顺序** 排序的整数数组 `nums`，返回 **每个数字的平方** 组成的新数组，要求也按 **非递减顺序** 排序。

**示例 1：**

```
输入：nums = [-4,-1,0,3,10]
输出：[0,1,9,16,100]
解释：平方后，数组变为 [16,1,0,9,100]
排序后，数组变为 [0,1,9,16,100]
```

**示例 2：**

```
输入：nums = [-7,-3,2,3,11]
输出：[4,9,9,49,121]
```

**提示：**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` 已按 **非递减顺序** 排序

**进阶：**

- 请你设计时间复杂度为 `O(n)` 的算法解决本问题

**解法一：得到平方数组并排序**

```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        for i in range(len(nums)):
            nums[i] *= nums[i]
        nums.sort()
        return nums
```

**解法二：双指针！**

双指针`i`和`j`从数组首尾开始遍历，取首尾处的平方最大值为当前数组平方最大值，放在结果数组`res`中，遍历完`nums`即可，循环条件是`i <= j`

```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        # 双指针法
        n = len(nums)
        i, j = 0, n-1
        k = n-1
        res = [0]*n
        # 考虑循环条件，i=j的时候，是上次循环中改变了i，j导致的
        # 故，i=j的时候，nums[i/j]还未处理，所以i=j也要进循环
        while i <= j:
            if nums[i]**2 > nums[j]**2:
                res[k] = nums[i]**2
                i += 1
            else:
                res[k] = nums[j]**2
                j -= 1
            k -= 1
        return res
```