### [764. 最大加号标志](https://leetcode.cn/problems/largest-plus-sign/)

- 中等

在一个 `n x n` 的矩阵 `grid` 中，除了在数组 `mines` 中给出的元素为 `0`，其他每个元素都为 `1`。`mines[i] = [xi, yi]`表示 `grid[xi][yi] == 0`

返回 `grid` *中包含 `1` 的最大的 **轴对齐** 加号标志的阶数* 。如果未找到加号标志，则返回 `0` 。

一个 `k` 阶由 *`1`* 组成的 **“轴对称”加号标志** 具有中心网格 `grid[r][c] == 1` ，以及4个从中心向上、向下、向左、向右延伸，长度为 `k-1`，由 `1` 组成的臂。注意，只有加号标志的所有网格要求为 `1` ，别的网格可能为 `0` 也可能为 `1` 。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2021/06/13/plus1-grid.jpg)

```
输入: n = 5, mines = [[4, 2]]
输出: 2
解释: 在上面的网格中，最大加号标志的阶只能是2。一个标志已在图中标出。
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2021/06/13/plus2-grid.jpg)

```
输入: n = 1, mines = [[0, 0]]
输出: 0
解释: 没有加号标志，返回 0 。
```

**提示：**

- `1 <= n <= 500`
- `1 <= mines.length <= 5000`
- `0 <= xi, yi < n`
- 每一对 `(xi, yi)` 都 **不重复**

**解法一：dp**

```python
class Solution:
    def orderOfLargestPlusSign(self, n: int, mines: List[List[int]]) -> int:
        # dp[i][j][k] 代表 (i, j) 处的 4 个方向上连续 1 的个数(包括grid[i][j])
        # 由于要取四个方向上的最小值，所以可以去掉第三维，直接保存最小值即可
        dp = [[n] * n for _ in range(n)]
        # 坐标映射成 tuple 的 set
        banned = set(map(tuple, mines))
        
        for i in range(n):
            # 为左边赋值，用 count 记录当前连续的 1 的个数
            count = 0
            for j in range(n):
                count = 0 if (i, j) in banned else count + 1
                dp[i][j] = min(dp[i][j], count)
            # 为右边赋值，从右往左遍历
            count = 0
            for j in range(n - 1, -1, -1):
                count = 0 if (i, j) in banned else count + 1
                dp[i][j] = min(dp[i][j], count)
        
        for j in range(n):
            # 为上边赋值，用 count 记录当前连续的 1 的个数
            count = 0
            for i in range(n):
                count = 0 if (i, j) in banned else count + 1
                dp[i][j] = min(dp[i][j], count)
            # 为下边赋值，从下往上遍历
            count = 0
            for i in range(n - 1, -1, -1):
                count = 0 if (i, j) in banned else count + 1
                dp[i][j] = min(dp[i][j], count)
        # map(max, dp) 是dp每一行的最大值
        # print(dp)
        # for i in map(max, dp):
        #     print(i)
        return max(map(max, dp))
```

