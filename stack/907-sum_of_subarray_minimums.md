### [907. 子数组的最小值之和](https://leetcode.cn/problems/sum-of-subarray-minimums/)

- 中等

给定一个整数数组 `arr`，找到 `min(b)` 的总和，其中 `b` 的范围为 `arr` 的每个（连续）子数组。

由于答案可能很大，因此 **返回答案模 `10^9 + 7`** 。

**示例 1：**

```
输入：arr = [3,1,2,4]
输出：17
解释：
子数组为 [3]，[1]，[2]，[4]，[3,1]，[1,2]，[2,4]，[3,1,2]，[1,2,4]，[3,1,2,4]。 
最小值为 3，1，2，4，1，1，2，1，1，1，和为 17。
```

**示例 2：**

```
输入：arr = [11,81,94,43,3]
输出：444
```

**提示：**

- `1 <= arr.length <= 3 * 10^4`
- `1 <= arr[i] <= 3 * 10^4`

**解法一：单调栈+数学**

考虑每个位置的元素，求以它为最小值的子数组位置，即要求：

**左边第一个小于它的值的索引，和右边第一个小于它的值索引，然后用其和当前索引差值的乘积**

由于可能存在连续相等的元素，且为最小值，如[5，2，3，3，3，2]，当遍历到第1，2，3个 3 时，会重复计算相同的情况

这时需要**左闭右开**考虑，即可以考虑，左边找到第一个更小的元素索引，右边找到第一个小于等于该元素的元素的索引，这时就不会重复计算了

要找一个元素左边，右边第一个更大/更小的元素，用单调栈

```python
class Solution:
    def sumSubarrayMins(self, arr: List[int]) -> int:
        n = len(arr)
        # l[i] 记录 arr[i] 左边第一个小于它的值的索引
        # r[i] 记录 arr[i] 右边第一个小于等于它的值的索引
        l, r = [-1] * n, [n] * n
        stack = []
        # 遍历数组，当前元素更小，就处理栈中元素
        for i in range(n):
            while stack and arr[i] <= arr[stack[-1]]:
                r[stack.pop()] = i
            stack.append(i)
        stack = []
        for i in range(n - 1, -1, -1):
            while stack and arr[i] < arr[stack[-1]]:
                l[stack.pop()] = i
            stack.append(i)
        ans = 0
        for i in range(n):
            a, b = i - l[i], r[i] - i
            ans += a * b * arr[i]
        return ans % (10**9 + 7)
```

**解法二：动态规划**

用二维动态规划，时间复杂度 $O(n^2)$ ，会超时

```python
class Solution:
    def sumSubarrayMins(self, arr: List[int]) -> int:
        # dp[i][j] 代表在 arr[i: j+1] 中的最小值
        # dp[i][i] = arr[i]
        # dp[i][j] = min(dp[i][j - 1], arr[j])
        n = len(arr)
        dp = [[0] * n for _ in range(n)]
        for i in range(n):
            dp[i][i] = arr[i]
        for i in range(n):
            for j in range(i + 1, n):
                dp[i][j] = min(dp[i][j - 1], arr[j])
        ans = 0
        for i in range(n):
            for j in range(i, n):
                ans += dp[i][j]
                ans = ans % (10**9 + 7)
        return ans
```