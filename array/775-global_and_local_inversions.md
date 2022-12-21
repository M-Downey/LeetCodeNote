### [775. 全局倒置与局部倒置](https://leetcode.cn/problems/global-and-local-inversions/)

- 中等

给你一个长度为 `n` 的整数数组 `nums` ，表示由范围 `[0, n - 1]` 内所有整数组成的一个排列。

**全局倒置** 的数目等于满足下述条件不同下标对 `(i, j)` 的数目：

- `0 <= i < j < n`
- `nums[i] > nums[j]`

**局部倒置** 的数目等于满足下述条件的下标 `i` 的数目：

- `0 <= i < n - 1`
- `nums[i] > nums[i + 1]`

当数组 `nums` 中 **全局倒置** 的数量等于 **局部倒置** 的数量时，返回 `true` ；否则，返回 `false` 。

**示例 1：**

```
输入：nums = [1,0,2]
输出：true
解释：有 1 个全局倒置，和 1 个局部倒置。
```

**示例 2：**

```
输入：nums = [1,2,0]
输出：false
解释：有 2 个全局倒置，和 1 个局部倒置。
```

**提示：**

- `n == nums.length`
- `1 <= n <= 10^5`
- `0 <= nums[i] < n`
- `nums` 中的所有整数 **互不相同**
- `nums` 是范围 `[0, n - 1]` 内所有数字组成的一个排列

**解法一：后缀最小值**

```python
class Solution:
    def isIdealPermutation(self, nums: List[int]) -> bool:
        # 局部倒置一定是全局倒置 -> 检查是否存在非局部倒置
        # 只要存在 i + 1 < j, nums[i] > nums[j] 就返回False
        # 检查 nums[j: n]的最小值，从后遍历，动态维护后缀的最小值，可以降低时间复杂度
        n = len(nums)
        # 从倒数第3个数开始检查
        minSuffix = nums[-1]
        for i in range(n - 3, -1, -1):
            if nums[i] > minSuffix:
                return False
            minSuffix = min(minSuffix, nums[i + 1])
        return True
```

