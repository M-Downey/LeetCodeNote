### [1034. 边界着色](https://leetcode.cn/problems/coloring-a-border/)

- 中等

给你一个大小为 `m x n` 的整数矩阵 `grid` ，表示一个网格。另给你三个整数 `row`、`col` 和 `color` 。网格中的每个值表示该位置处的网格块的颜色。

两个网格块属于同一 **连通分量** 需满足下述全部条件：

- 两个网格块颜色相同
- 在上、下、左、右任意一个方向上相邻

**连通分量的边界** 是指连通分量中满足下述条件之一的所有网格块：

- 在上、下、左、右任意一个方向上与不属于同一连通分量的网格块相邻
- 在网格的边界上（第一行/列或最后一行/列）

请你使用指定颜色 `color` 为所有包含网格块 `grid[row][col]` 的 **连通分量的边界** 进行着色，并返回最终的网格 `grid` 。

**示例 1：**

```
输入：grid = [[1,1],[1,2]], row = 0, col = 0, color = 3
输出：[[3,3],[3,2]]
```

**示例 2：**

```
输入：grid = [[1,2,2],[2,3,2]], row = 0, col = 1, color = 3
输出：[[1,3,3],[2,3,3]]
```

**示例 3：**

```
输入：grid = [[1,1,1],[1,1,1],[1,1,1]], row = 1, col = 1, color = 2
输出：[[2,2,2],[2,1,2],[2,2,2]]
```

**提示：**

- `m == grid.length`
- `n == grid[i].length`

- `1 <= m, n <= 50`
- `1 <= grid[i][j], color <= 1000`

- `0 <= row < m`
- `0 <= col < n`

 **解法一：DFS**

`DFS` 遍历，把边界点坐标记录下来，然后再遍历边界点上色

对于一个点 `(row, col)` ， 怎么判断它是边界点呢？

判断它周围的点是否是岛内点，即：坐标合法，颜色相同。如果不是的话，它就是边界点，要记录下来

同时，对于岛内点要继续 `DFS` 

```python
class Solution:
    def colorBorder(self, grid: List[List[int]], row: int, col: int, color: int) -> List[List[int]]:
        # 如果当前岛屿颜色等于 color 不用上色
        if grid[row][col] == color:
            return grid
        m, n = len(grid), len(grid[0])
        visited = [[False for _ in range(n)] for _ in range(m)]
        borders = []
        self.dfs(grid, row, col, grid[row][col], visited, borders)
        # print(borders)
        for x, y in borders:
            grid[x][y] = color
        return grid

    def dfs(self, grid, row, col, color, visited, borders):
        m, n = len(grid), len(grid[0])
        visited[row][col] = True
        isBorder = False
        for r, c in [(row - 1, col), (row + 1, col), (row, col - 1), (row, col + 1)]:
            if 0 <= r < m and 0 <= c < n and grid[r][c] == color:
                if visited[r][c] == False:
                    self.dfs(grid, r, c, color, visited, borders)
            else:
                isBorder = True
        if isBorder:
            borders.append((row, col))
```

