### [695. 岛屿的最大面积](https://leetcode.cn/problems/max-area-of-island/)

- 中等

给你一个大小为 `m x n` 的二进制矩阵 `grid` 。

**岛屿** 是由一些相邻的 `1` (代表土地) 构成的组合，这里的「相邻」要求两个 `1` 必须在 **水平或者竖直的四个方向上** 相邻。你可以假设 `grid` 的四个边缘都被 `0`（代表水）包围着。

岛屿的面积是岛上值为 `1` 的单元格的数目。

计算并返回 `grid` 中最大的岛屿面积。如果没有岛屿，则返回面积为 `0` 。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2021/05/01/maxarea1-grid.jpg)

```
输入：grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
输出：6
解释：答案不应该是 11 ，因为岛屿只能包含水平或垂直这四个方向上的 1 。
```

**示例 2：**

```
输入：grid = [[0,0,0,0,0,0,0,0]]
输出：0
```

**提示：**

- `m == grid.length`
- `n == grid[i].length`

- `1 <= m, n <= 50`
- `grid[i][j]` 为 `0` 或 `1`

**解法一：DFS**

遍历每个岛屿， `DFS` 求每个岛屿的面积，保存最大值

```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        maxArea = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    tmpArea = self.dfs(grid, i, j)
                    if maxArea < tmpArea:
                        maxArea = tmpArea
        return maxArea
    
    def dfs(self, grid, row, col):
        grid[row][col] = 0
        area = 1
        m, n = len(grid), len(grid[0])
        for r, c in [(row - 1, col), (row + 1, col), (row, col - 1), (row, col + 1)]:
            if 0 <= r < m and 0 <= c < n and grid[r][c] == 1:
                area += self.dfs(grid, r, c)
        return area
```

