### [400. 第 N 位数字](https://leetcode.cn/problems/nth-digit/)

- 中等

给你一个整数 `n` ，请你在无限的整数序列 `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...]` 中找出并返回第 `n` 位上的数字。

**示例 1：**

```
输入：n = 3
输出：3
```

**示例 2：**

```
输入：n = 11
输出：0
解释：第 11 位数字在序列 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... 里是 0 ，它是 10 的一部分。
```

**提示：**

- `1 <= n <= 2^31 - 1`

**解法一：二分**

考虑规律：

- 一位数的数位为 9 * 1
- 二位数的数位为 90 * 2
- 三位数的数位为 900 * 3
- ...
- n位数的数位为 $9 * 10^{n-1} * n$

再考虑所有数位和的前缀和数组（代码用函数实现了），找到第一个大于等于 n 的位置

然后再考虑在这个 x 位数中找到第 n - preSum的位置

```python
class Solution:
    def findNthDigit(self, n: int) -> int:
        # 得到长小于等于 length 的数字的所有位数之和
        def get_digits(length):
            nums = 0
            # 长为 i 位的数有 i * 9 * 10**(i - 1)
            for i in range(1, length + 1):
                nums += i * 9 * 10 **(i - 1)
            return nums
        # for i in range(9):
        #     print(get_digits(i + 1))
        # 找到 n 是在几位数中出现的
        left, right = 1, 9
        while left < right:
            mid = (left + right) // 2
            if get_digits(mid) < n:
                left = mid + 1
            else:
                right = mid
        # 前面位数 <= left 的所有数的位数之和
        preSum = get_digits(left - 1)
        # 每个数是 left 位
        start = 10 ** (left - 1)
        # 第几位数，从 0 开始
        idx = n - preSum - 1
        num = start + idx // left
        # 计算第 n 位在数 num 中右边有几位
        right_digits = left - idx % left - 1
        return num // 10**right_digits % 10
```

