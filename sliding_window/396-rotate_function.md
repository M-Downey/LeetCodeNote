### [396. 旋转函数](https://leetcode.cn/problems/rotate-function/)

- 中等

给定一个长度为 `n` 的整数数组 `nums` 。

假设 `arrk` 是数组 `nums` 顺时针旋转 `k` 个位置后的数组，我们定义 `nums` 的 **旋转函数** `F` 为：

- `F(k) = 0 * arrk[0] + 1 * arrk[1] + ... + (n - 1) * arrk[n - 1]`

返回 *`F(0), F(1), ..., F(n-1)`中的最大值* 。

生成的测试用例让答案符合 **32 位** 整数。

**示例 1:**

```
输入: nums = [4,3,2,6]
输出: 26
解释:
F(0) = (0 * 4) + (1 * 3) + (2 * 2) + (3 * 6) = 0 + 3 + 4 + 18 = 25
F(1) = (0 * 6) + (1 * 4) + (2 * 3) + (3 * 2) = 0 + 4 + 6 + 6 = 16
F(2) = (0 * 2) + (1 * 6) + (2 * 4) + (3 * 3) = 0 + 6 + 8 + 9 = 23
F(3) = (0 * 3) + (1 * 2) + (2 * 6) + (3 * 4) = 0 + 2 + 12 + 12 = 26
所以 F(0), F(1), F(2), F(3) 中的最大值是 F(3) = 26 。
```

**示例 2:**

```
输入: nums = [100]
输出: 0
```

**提示:**

- `n == nums.length`
- `1 <= n <= 10^5`
- `-100 <= nums[i] <= 100`

**解法一：滑动窗口**

数组复制后，用长为 `n` 的滑动窗口遍历，每次计算一个值，然后比较记录答案即可

但复杂度为 $O(n^2)$ 会超时

```python
class Solution:
    def maxRotateFunction(self, nums: List[int]) -> int:
        n = len(nums)
        nums += nums
        ans = float('-inf')
        def func(array):
            res = 0
            for i, num in enumerate(array):
                res += num * i
            return res 
        # 从第 n 位计算到第 1 位，每次计算 n 个元素
        for i in range(n, 0, -1):
            ans = max(ans, func(nums[i: i + n]))
        return ans
```

**解法二：滑动窗口 + 前缀和**

每次后移一个窗口，会多一个新值，以及前面的每一个值都会少计算一次（相对前一个窗口的值来说），所以可以用前缀和计算这一差值

后一窗口的值可以用前一窗口，加上新的窗口最右处的值，减去前 `n-1` 个元素的和来计算

```python
class Solution:
    def maxRotateFunction(self, nums: List[int]) -> int:
        n = len(nums)
        nums += nums
        preSum = [0] * (2 * n + 1)
        for i in range(1, 2 * n + 1):
            preSum[i] = preSum[i - 1] + nums[i - 1]
        ans = 0
        for i in range(n):
            ans += i * nums[i]
        tmp = ans
        for i in range(n, 2 * n - 1):
            tmp += nums[i] * (n - 1)
            # 索引i - 1 到 i - n + 1 之间 (n-1) 个元素
            # 也就是第 i - n + 2 到 第 i 之间
            tmp -= preSum[i] - preSum[i - n + 1]
            if tmp > ans:
                ans = tmp
        return ans
```

优化代码，不拼接 `nums` 数组，只调整下标的话

```python
class Solution:
    def maxRotateFunction(self, nums: List[int]) -> int:
        n = len(nums)
        preSum = [0] * (2 * n + 1)
        # preSum[i] 前 i 个元素的和
        for i in range(1, 2 * n + 1):
            preSum[i] = preSum[i - 1] + nums[(i - 1) % n]
        ans = 0
        for i in range(n):
            ans += i * nums[i]
        tmp = ans
        # 向右移动窗口，更新
        for i in range(n, 2 * n - 1):
            tmp += nums[i % n] * (n - 1)
            tmp -= preSum[i] - preSum[i - n + 1]
            if tmp > ans:
                ans = tmp
        return ans
```

