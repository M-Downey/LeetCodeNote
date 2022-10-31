### [2104. 子数组范围和](https://leetcode.cn/problems/sum-of-subarray-ranges/)

- 中等

给你一个整数数组 `nums` 。`nums` 中，子数组的 **范围** 是子数组中最大元素和最小元素的差值。

返回 `nums` 中 **所有** 子数组范围的 **和** *。*

子数组是数组中一个连续 **非空** 的元素序列。

**示例 1：**

```
输入：nums = [1,2,3]
输出：4
解释：nums 的 6 个子数组如下所示：
[1]，范围 = 最大 - 最小 = 1 - 1 = 0 
[2]，范围 = 2 - 2 = 0
[3]，范围 = 3 - 3 = 0
[1,2]，范围 = 2 - 1 = 1
[2,3]，范围 = 3 - 2 = 1
[1,2,3]，范围 = 3 - 1 = 2
所有范围的和是 0 + 0 + 0 + 1 + 1 + 2 = 4
```

**示例 2：**

```
输入：nums = [1,3,3]
输出：4
解释：nums 的 6 个子数组如下所示：
[1]，范围 = 最大 - 最小 = 1 - 1 = 0
[3]，范围 = 3 - 3 = 0
[3]，范围 = 3 - 3 = 0
[1,3]，范围 = 3 - 1 = 2
[3,3]，范围 = 3 - 3 = 0
[1,3,3]，范围 = 3 - 1 = 2
所有范围的和是 0 + 0 + 0 + 2 + 0 + 2 = 4
```

**示例 3：**

```
输入：nums = [4,-2,-3,4,1]
输出：59
解释：nums 中所有子数组范围的和是 59
```

**提示：**

- `1 <= nums.length <= 1000`
- `-10^9 <= nums[i] <= 10^9`

**进阶：**你可以设计一种时间复杂度为 `O(n)` 的解决方案吗？

**解法一：单调栈+数学**

某个元素 `nums[i]` 对答案的贡献有两种，作为最大值和作为最小值

可以计算 `nums[i]` 作为最大值的区间个数和作为最小值的区间个数，再作差乘以 `nums[i]` 即可

要计算 `nums[i]` 作为最大值或最小值的区间个数，利用数学和单调栈，找某个元素两边更大或更小的元素即可

注意为避免重复计算某些情况，要**左开右闭**或**左闭右开**

```python
class Solution:
    def subArrayRanges(self, nums: List[int]) -> int:
        # 求某个元素对答案的贡献
        # nums[i] 作为区间最大值的个数和区间最小值的个数，相减，再乘 nums[i]
        ans = 0
        M, m = self.getCnt(nums, True), self.getCnt(nums, False)
        n = len(nums)
        for i in range(n):
            ans += (M[i] - m[i]) * nums[i]
        return ans

    # 获得 nums[i] 为最大或最小值的区间的个数
    def getCnt(self, nums, isMax=True):
        n = len(nums)
        l, r = [-1] * n, [n] * n
        stack = []
        for i in range(n):
            # 作为最大值，要求两边第一个更大的值
            if isMax:
                while stack and nums[i] >= nums[stack[-1]]:
                    r[stack.pop()] = i
            # 作为最小值，要求两边第一个更小的值
            else:
                while stack and nums[i] <= nums[stack[-1]]:
                    r[stack.pop()] = i
            stack.append(i)
        for i in range(n - 1, -1, -1):
            if isMax:
                while stack and nums[i] > nums[stack[-1]]:
                    l[stack.pop()] = i
            else:
                while stack and nums[i] < nums[stack[-1]]:
                    l[stack.pop()] = i
            stack.append(i)
        ans = []
        for i in range(n):
            ans.append((i - l[i]) * (r[i] - i))
        return ans
```

