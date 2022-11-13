### [152. 乘积最大子数组](https://leetcode.cn/problems/maximum-product-subarray/)

- 中等

给你一个整数数组 `nums` ，请你找出数组中乘积最大的非空连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

测试用例的答案是一个 **32-位** 整数。

**子数组** 是数组的连续子序列。

**示例 1:**

```
输入: nums = [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
```

**示例 2:**

```
输入: nums = [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。
```

**提示:**

- `1 <= nums.length <= 2 * 10^4`
- `-10 <= nums[i] <= 10`
- `nums` 的任何前缀或后缀的乘积都 **保证** 是一个 **32-位** 整数

**解法一：dp**

最初只考虑了 `dp[i] = max(dp[i - 1] * nums[i], nums[i])` 是不对的，因为有负数存在，可能到 `i-1` 处子数组乘积是负数， `nums[i]` 也是负数，这样就可以得到一个最大值了，所以在记录最大连续子数组乘积时，还需记录最小连续子数组乘积

```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        # dp[i] 代表以 nums[i] 结尾的连续子数组最大积
        # Min[i] 代表以 nums[i] 结尾的连续子数组最小积
        # dp[i] = max(dp[i - 1] * nums[i], Min[i - 1] * nums[i], nums[i])
        # Min[i] = min(dp[i - 1] * nums[i], Min[i - 1] * nums[i], nums[i])
        # [-2,3,-4]
        n = len(nums)
        dp = nums[: ]
        Min = nums[: ]
        for i in range(1, n):
            dp[i] = max(dp[i - 1] * nums[i], Min[i - 1] * nums[i], nums[i])
            Min[i] = min(dp[i - 1] * nums[i], Min[i - 1] * nums[i], nums[i])
        # print(dp)
        # print(Min)
        return max(dp)
```

