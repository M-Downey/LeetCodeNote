### [967. 连续差相同的数字](https://leetcode.cn/problems/numbers-with-same-consecutive-differences/)

- 中等

返回所有长度为 `n` 且满足其每两个连续位上的数字之间的差的绝对值为 `k` 的 **非负整数** 。

请注意，**除了** 数字 `0` 本身之外，答案中的每个数字都 **不能** 有前导零。例如，`01` 有一个前导零，所以是无效的；但 `0` 是有效的。

你可以按 **任何顺序** 返回答案。

**示例 1：**

```
输入：n = 3, k = 7
输出：[181,292,707,818,929]
解释：注意，070 不是一个有效的数字，因为它有前导零。
```

**示例 2：**

```
输入：n = 2, k = 1
输出：[10,12,21,23,32,34,43,45,54,56,65,67,76,78,87,89,98]
```

**示例 3：**

```
输入：n = 2, k = 0
输出：[11,22,33,44,55,66,77,88,99]
```

**示例 4：**

```
输入：n = 2, k = 2
输出：[13,20,24,31,35,42,46,53,57,64,68,75,79,86,97]
```

**提示：**

- `2 <= n <= 9`
- `0 <= k <= 9`

**解法一：回溯**

```python
class Solution:
    def __init__(self):
        self.path = []
        self.ans = []
        self.valid = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

    def numsSameConsecDiff(self, n: int, k: int) -> List[int]:
        # 搜索树有 n 层，每层选一个数字，到达第 n 层记录答案
        for i in range(1, 10):
            self.path.append(i)
            self.backtracking(n, k, 1)
            self.path.pop()
        return self.ans

    def backtracking(self, n, k, index):
        if len(self.path) == n:
            ans = int(''.join(str(i) for i in self.path))
            self.ans.append(ans)
            return

        next_nums = []
        a, b = self.path[-1] + k, self.path[-1] - k
        if a in self.valid:
            next_nums.append(a)
        if b in self.valid:
            # 如果 k=0，那么 a=b ，这种情况不能添加两个一样的数字
            if a != b:
                next_nums.append(b)

        for i in next_nums:
            self.path.append(i)
            self.backtracking(n, k, index + 1)
            self.path.pop()
```

