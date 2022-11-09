### [740. 删除并获得点数](https://leetcode.cn/problems/delete-and-earn/)

- 中等

给你一个整数数组 `nums` ，你可以对它进行一些操作。

每次操作中，选择任意一个 `nums[i]` ，删除它并获得 `nums[i]` 的点数。之后，你必须删除 **所有** 等于 `nums[i] - 1` 和 `nums[i] + 1` 的元素。

开始你拥有 `0` 个点数。返回你能通过这些操作获得的最大点数。

**示例 1：**

```
输入：nums = [3,4,2]
输出：6
解释：
删除 4 获得 4 个点数，因此 3 也被删除。
之后，删除 2 获得 2 个点数。总共获得 6 个点数。
```

**示例 2：**

```
输入：nums = [2,2,3,3,3,4]
输出：9
解释：
删除 3 获得 3 个点数，接着要删除两个 2 和 4 。
之后，再次删除 3 获得 3 个点数，再次删除 3 获得 3 个点数。
总共获得 9 个点数。
```

**提示：**

- `1 <= nums.length <= 2 * 10^4`
- `1 <= nums[i] <= 10^4`

**解法一：哈希+dp**

和 [198-打家劫舍](https://leetcode.cn/problems/house-robber/) 一样

选择了一个 `nums[i]` ，就不能取它相邻的值，并且要把所有的 `nums[i]` 都加到结果里

所以可以用 `num: num_sum` 的映射，构建一个新的数组 `values`

对于 `values` 来说，要“打家劫舍”，但是不能拿相邻的房子的钱

下面用 `dp` 就好了

```python
class Solution:
    def deleteAndEarn(self, nums: List[int]) -> int:
        # 选择了 nums[i] ，相当于能把所有 nums[i] 都计入答案
        maxVal = max(nums)
        values = [0] * (maxVal + 1)
        for num in nums:
            values[num] += num
        dp = [0] * (maxVal + 1)
        # dp[i] 代表偷到第 i 家的最大收益
        # dp[i] = max(values[i] + dp[i - 2], dp[i - 1])
        dp[0] = values[0]
        dp[1] = max(values[0], values[1])
        for i in range(2, maxVal + 1):
            dp[i] = max(values[i] + dp[i - 2], dp[i - 1])
        # print(dp)
        return dp[maxVal]
```

