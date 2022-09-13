### [670. 最大交换](https://leetcode.cn/problems/maximum-swap/)

- 中等

给定一个非负整数，你**至多**可以交换一次数字中的任意两位。返回你能得到的最大值。

**示例 1 :**

```
输入: 2736
输出: 7236
解释: 交换数字2和数字7。
```

**示例 2:**

```
输入: 9973
输出: 9973
解释: 不需要交换。
```

**注意:**

1. 给定数字的范围是 [0, 10^8]

**解法一：贪心**

从左遍历，然后再它的右边（从后向前遍历）找到第一个比它大的最大值，进行交换

```python
class Solution:
    def maximumSwap(self, num: int) -> int:
        ls = [int(char) for char in str(num)]
        n = len(ls)
        if n <= 1:
            return num
        
        for i in range(n - 1):
            tmp = 0
            idx = 0
            for j in range(n - 1, i, -1):
                if ls[j] > tmp:
                    tmp = ls[j]
                    idx = j
            # print(tmp, idx)
            # print(ls[i])
            if tmp > ls[i]:
                ls[i], ls[idx] = ls[idx], ls[i]
                break
        ans = int(''.join([str(char) for char in ls]))
        return ans
```

