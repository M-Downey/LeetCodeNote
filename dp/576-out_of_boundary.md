### [576. 出界的路径数](https://leetcode.cn/problems/out-of-boundary-paths/)
- 中等

给你一个大小为 `m x n` 的网格和一个球。球的起始坐标为 `[startRow, startColumn]` 。你可以将球移到在四个方向上相邻的单元格内（可以穿过网格边界到达网格之外）。你 **最多** 可以移动 `maxMove` 次球。

给你五个整数 `m、n、maxMove、startRow` 以及 `startColumn `，找出并返回可以将球移出边界的路径数量。因为答案可能非常大，返回对 `10^9 + 7` **取余** 后的结果。

**示例 1：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/8d4265e5e8e64386974a82904416b944.png)

```
输入：m = 2, n = 2, maxMove = 2, startRow = 0, startColumn = 0
输出：6
```

**示例 2：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/5c6cc875cc094c8a9eb957f4283f6373.png)
```
输入：m = 1, n = 3, maxMove = 3, startRow = 0, startColumn = 1
输出：12
```
**提示：**
- `1 <= m, n <= 50`
- `0 <= maxMove <= 50`
- `0 <= startRow < m`
- `0 <= startColumn < n`

**解法一：动态规划**
```python
class Solution:
    def findPaths(self, m: int, n: int, maxMove: int, startRow: int, startColumn: int) -> int:
        # dp[i][j][k] 代表走了 i 步到达 (j, k) 的路径数
        # 对于第 i+1 步，只可能是在 (j, k) 的四个方向上
        # 考虑周围的四对坐标，如果是合法的，就相当于增加了一些到 (j', k') 的路径，加上即可
        # 不合法，就把 dp[i][j][k] 加到总路径数中
        MOD = 10**9 + 7
        ans = 0
        dp = [[[0] * n for _ in range(m)] for _ in range(maxMove + 1)]
        dp[0][startRow][startColumn] = 1
        for i in range(maxMove):
            for j in range(m):
                for k in range(n):
                    for nj, nk in [(j + 1, k), (j - 1, k), (j, k + 1), (j, k - 1)]:
                        if 0 <= nj < m and 0 <= nk < n:
                            dp[i + 1][nj][nk] = (dp[i + 1][nj][nk] + dp[i][j][k]) % MOD
                        else:
                            ans = (ans + dp[i][j][k]) % MOD
        return ans
```