### [994. 腐烂的橘子](https://leetcode.cn/problems/rotting-oranges/)

- 中等

在给定的 `m x n` 网格 `grid` 中，每个单元格可以有以下三个值之一：

- 值 `0` 代表空单元格；
- 值 `1` 代表新鲜橘子；
- 值 `2` 代表腐烂的橘子。

每分钟，腐烂的橘子 **周围 4 个方向上相邻** 的新鲜橘子都会腐烂。

返回 *直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 `-1`* 。

**示例 1：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/16/oranges.png)

```
输入：grid = [[2,1,1],[1,1,0],[0,1,1]]
输出：4
```

**示例 2：**

```
输入：grid = [[2,1,1],[0,1,1],[1,0,1]]
输出：-1
解释：左下角的橘子（第 2 行， 第 0 列）永远不会腐烂，因为腐烂只会发生在 4 个正向上。
```

**示例 3：**

```
输入：grid = [[0,2]]
输出：0
解释：因为 0 分钟时已经没有新鲜橘子了，所以答案就是 0 。
```

**提示：**

- `m == grid.length`
- `n == grid[i].length`

- `1 <= m, n <= 10`
- `grid[i][j]` 仅为 `0`、`1` 或 `2`

**解法一：BFS**

`BFS` 遍历烂橘子周围，用新一层的烂橘子更新队列，每次更新队列的话，就把分钟数加一，并把橘子变烂。

最后检查如果还有新鲜橘子，就返回 `-1`

否则，返回分钟数

```python
class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        # 把烂橘子周围的橘子都入队
        m, n = len(grid), len(grid[0])
        visited = [[False for _ in range(n)] for _ in range(m)]
        q = []
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 2:
                    visited[i][j] = True
                    q.append((i, j))
        mins = 0
        while q:
            tmp = []
            # print(q)
            for row, col in q:
                for r, c in [(row - 1, col), (row + 1, col), (row, col - 1), (row, col + 1)]:
                    if 0 <= r < m and 0 <= c < n and visited[r][c] == False and grid[r][c] == 1:
                        grid[r][c] = 2
                        visited[r][c] = True
                        tmp.append((r, c))
            if tmp:
                mins += 1
            q = tmp
        # 检查是否有新鲜橘子
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    return -1
        return mins
```

