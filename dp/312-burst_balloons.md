### [312. 戳气球](https://leetcode.cn/problems/burst-balloons/)

- 困难

有 `n` 个气球，编号为`0` 到 `n - 1`，每个气球上都标有一个数字，这些数字存在数组 `nums` 中。

现在要求你戳破所有的气球。戳破第 `i` 个气球，你可以获得 `nums[i - 1] * nums[i] * nums[i + 1]` 枚硬币。 这里的 `i - 1` 和 `i + 1` 代表和 `i` 相邻的两个气球的序号。如果 `i - 1`或 `i + 1` 超出了数组的边界，那么就当它是一个数字为 `1` 的气球。

求所能获得硬币的最大数量。

 **示例 1：**

```
输入：nums = [3,1,5,8]
输出：167
解释：
nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
coins =  3*1*5    +   3*5*8   +  1*3*8  + 1*8*1 = 167
```

**示例 2：**

```
输入：nums = [1,5]
输出：10
```

**提示：**

- `n == nums.length`
- `1 <= n <= 300`
- `0 <= nums[i] <= 100`

**解法一：区间dp**

```python
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        # dp[i][j] 代表戳完开区间 (i, j) 气球的最大收益
        # dp[0][n + 1] 为答案
        # dp[i][j]可以分为 (i, k) 和 (k, j) 两个区间，遍历 i, j 之间的所有 k 得到最大的收益
        # dp[i][j] = max(dp[i][k] + dp[k][j] + nums[i] * nums[k] * nums[j])
        # 只需上半矩阵, 且从下往上赋值(因为dp[i][j]需要更小的区间的值), dp[i][i] 和 dp[i][i+1] 都默认为0
        n = len(nums)
        nums = [1] + nums + [1]
        dp = [[0] * (n + 2) for _ in range(n + 2)]
        # 直接从倒数第三行开始赋值, 倒数两行的3个值都是0
        for i in range(n - 1, -1, -1):
            for j in range(i + 2, n + 2):
                # k 是 (i, j) 中的所有分割点
                for k in range(i + 1, j):
                    tmp = nums[i] * nums[k] * nums[j] + dp[i][k] + dp[k][j]
                    dp[i][j] = max(dp[i][j], tmp)
        return dp[0][n + 1]
```

