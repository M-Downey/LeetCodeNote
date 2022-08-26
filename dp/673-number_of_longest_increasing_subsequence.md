### [673. 最长递增子序列的个数](https://leetcode.cn/problems/number-of-longest-increasing-subsequence/)

- 中等

给定一个未排序的整数数组 `nums` ， *返回最长递增子序列的个数* 。

**注意** 这个数列必须是 **严格** 递增的。

**示例 1:**

```
输入: [1,3,5,4,7]
输出: 2
解释: 有两个最长递增子序列，分别是 [1, 3, 4, 7] 和[1, 3, 5, 7]。
```

**示例 2:**

```
输入: [2,2,2,2,2]
输出: 5
解释: 最长递增子序列的长度是1，并且存在5个子序列的长度为1，因此输出5。
```

**提示:** 

- `1 <= nums.length <= 2000`
- `-10^6 <= nums[i] <= 10^6`

**解法一：动规**

```python
class Solution:
    def findNumberOfLIS(self, nums: List[int]) -> int:
        # dp[i] 代表以 nums[i] 结尾的最长子序列的长度
        # count[i] 代表以 nums[i] 结尾的最长子序列的个数
        # dp[i] = max(dp[i], dp[j] + 1) for j in range(i)
        # count[i] = count[j] if dp[j] + 1 > dp[i] 找到了更长的子序列，那么重置子序列的个数
        # count[i] += count[j] if dp[j] + 1 == dp[i] 找到了一样长的子序列
        n = len(nums)
        dp = [1] * n
        count = [1] * n
        maxLen = 0
        for i in range(n):
            for j in range(i):
                if nums[i] > nums[j]:
                    if dp[j] + 1 > dp[i]:
                        count[i] = count[j]
                    elif dp[j] + 1 == dp[i]:
                        count[i] += count[j]
                    dp[i] = max(dp[i], dp[j] + 1)
            if dp[i] > maxLen:
                maxLen = dp[i]
        ans = 0
        for i in range(n):
            if dp[i] == maxLen:
                ans += count[i]
        return ans
```

