### [878. 第 N 个神奇数字](https://leetcode.cn/problems/nth-magical-number/)

- 困难

一个正整数如果能被 `a` 或 `b` 整除，那么它是神奇的。

给定三个整数 `n` , `a` , `b` ，返回第 `n` 个神奇的数字。因为答案可能很大，所以返回答案 **对** `109 + 7` **取模** 后的值。

**示例 1：**

```
输入：n = 1, a = 2, b = 3
输出：2
```

**示例 2：**

```
输入：n = 4, a = 2, b = 3
输出：6
```

**提示：**

- `1 <= n <= 10^9`
- `2 <= a, b <= 4 * 10^4`

**解法一：二分查找**

```python
class Solution:
    def nthMagicalNumber(self, n: int, a: int, b: int) -> int:
        # f(x) 表示小于等于 x 的神奇数的个数 = a的倍数的个数+b的倍数的数减去 lcm(a, b)的倍数的个数
        # 从 min(a, b) 到 n*min(a, b) 的连续整数范围中找到第n个
        # 二分查找，只要大于等于n，就要继续往左找
        # 最后一定是 r + 1 = l
        # 而 l-1 一定是小于 n 的，所以返回 r+1，也就是l
        MOD = 10 ** 9 + 7
        l = min(a, b)
        r = n * min(a, b)
        # 最小公倍数
        c = lcm(a, b)
        while l <= r:
            mid = (l + r) // 2
            cnt = mid // a + mid // b - mid // c
            if cnt >= n:
                r = mid - 1
            else:
                l = mid + 1
        return l % MOD
```

