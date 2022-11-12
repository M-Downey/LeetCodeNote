### [790. 多米诺和托米诺平铺](https://leetcode.cn/problems/domino-and-tromino-tiling/)

- 中等

有两种形状的瓷砖：一种是 `2 x 1` 的多米诺形，另一种是形如 "L" 的托米诺形。两种形状都可以旋转。

 ![img](https://assets.leetcode.com/uploads/2021/07/15/lc-domino.jpg)

给定整数 n ，返回可以平铺 `2 x n` 的面板的方法的数量。**返回对** `109 + 7` **取模** 的值。

平铺指的是每个正方形都必须有瓷砖覆盖。两个平铺不同，当且仅当面板上有四个方向上的相邻单元中的两个，使得恰好有一个平铺有一个瓷砖占据两个正方形。

**示例 1:**

 ![img](https://assets.leetcode.com/uploads/2021/07/15/lc-domino1.jpg)

```
输入: n = 3
输出: 5
解释: 五种不同的方法如上所示。
```

**示例 2:**

```
输入: n = 1
输出: 1
```

**提示：**

- `1 <= n <= 1000`

**解法一：DP**

`dp` 前驱状态之间要**全面且不重叠**

```python
class Solution:
    def numTilings(self, n: int) -> int:
        # dp[i][j] 代表第 i 列填充的情况，前 i - 1 列已填充满，第 i 列填充情况有4种
        # j = 0, 1, 2, 3 分别代表空，上1，下1，满 四种情况
        # dp[i][0] = dp[i - 1][3]
        # dp[i][1] = dp[i - 1][0] + dp[i - 1][2]
        # dp[i][2] = dp[i - 1][0] + dp[i - 1][1]
        # dp[i][3] = dp[i - 1][2] + dp[i - 1][1] + dp[i - 1][2] + dp[i - 1][0]
        dp = [[0] * 4 for _ in range(n + 1)]
        # 第一列空也是有一种情况的
        dp[1][0] = 1
        dp[1][3] = 1
        MOD = 10 ** 9 + 7
        for i in range(2, n + 1):
            dp[i][0] = dp[i - 1][3]
            dp[i][1] = (dp[i - 1][0] + dp[i - 1][2]) % MOD
            dp[i][2] = (dp[i - 1][0] + dp[i - 1][1]) % MOD
            dp[i][3] = (dp[i - 1][3] + dp[i - 1][1] + dp[i - 1][2] + dp[i - 1][0]) % MOD
        # print(dp)
        return dp[n][3]
```

