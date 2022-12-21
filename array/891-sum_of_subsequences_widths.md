### [891. 子序列宽度之和](https://leetcode.cn/problems/sum-of-subsequence-widths/)

- 困难

一个序列的 **宽度** 定义为该序列中最大元素和最小元素的差值。

给你一个整数数组 `nums` ，返回 `nums` 的所有非空 **子序列** 的 **宽度之和** 。由于答案可能非常大，请返回对 `109 + 7` **取余** 后的结果。

**子序列** 定义为从一个数组里删除一些（或者不删除）元素，但不改变剩下元素的顺序得到的数组。例如，`[3,6,2,7]` 就是数组 `[0,3,1,6,2,2,7]` 的一个子序列。

**示例 1：**

```
输入：nums = [2,1,3]
输出：6
解释：子序列为 [1], [2], [3], [2,1], [2,3], [1,3], [2,1,3] 。
相应的宽度是 0, 0, 0, 1, 1, 2, 2 。
宽度之和是 6 。
```

**示例 2：**

```
输入：nums = [2]
输出：0
```

**提示：**

- `1 <= nums.length <= 10^5`
- `1 <= nums[i] <= 10^5`

**解法一：数学**

将 `nums` 排序后，求以 `nums[j]` 为结尾的所有子序列的宽度之和，记为 $B_j$
$$
\begin{align}
B_j &= \sum_{i < j}(nums[j] - nums[i]) \times 2^{j - i - 1}\\
	&= nums[j] \times \sum_{i < j}2^{j - i - 1} - \sum_{i < j} nums[i] \times 2^{j - i - 1}\\
	&= nums[j] \times (2^j - 1) - \sum_{i < j} nums[i] \times 2^{j - i - 1}
\end{align}
$$
设 $x_i = \sum_{i < j}nums[i] \times 2^{j - i - 1}$ ，则
$$
\begin{align}
x_{j + 1} &= \sum_{i < j + 1} nums[i] \times 2^{j - i}\\
		  &= \sum_{i < j} nums[i] \times 2^{j - 1} + nums[j]\\
		  &= 2 \times x_i + nums[j]
\end{align}
$$
对遍历 `j` 对 $B_j$ 累加求和，同时更新 $x_j$ 和 $2^j$ 

```python
class Solution:
    def sumSubseqWidths(self, nums: List[int]) -> int:
        # 子序列的顺序不影响子序列的宽度，所以先排序
        # 统计以 nums[j](j >= 1) 结尾的所有子序列的宽度之和
        # 考虑每个 i (i < j) nums[i], nums[j] 之间以其为最值的子序列个数是 2^(j - i - 1)
        # 宽度是 (nums[j] - nums[i]) * 2^(j - i - 1) 对 i 求和
        MOD = 10 ** 9 + 7
        ans = 0
        nums.sort()
        x, y = nums[0], 2
        n = len(nums)
        for j in range(1, n):
            ans = (ans + nums[j] * (y - 1) - x) % MOD
            x = (x * 2 + nums[j]) % MOD
            y = (y * 2) % MOD
        return ans
```



