### [1254. 统计封闭岛屿的数目](https://leetcode.cn/problems/number-of-closed-islands/)

- 中等

二维矩阵 `grid` 由 `0` （土地）和 `1` （水）组成。岛是由最大的4个方向连通的 `0` 组成的群，封闭岛是一个 `完全` 由1包围（左、上、右、下）的岛。

请返回 *封闭岛屿* 的数目。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2019/10/31/sample_3_1610.png)

```
输入：grid = [[1,1,1,1,1,1,1,0],[1,0,0,0,0,1,1,0],[1,0,1,0,1,1,1,0],[1,0,0,0,0,1,0,1],[1,1,1,1,1,1,1,0]]
输出：2
解释：
灰色区域的岛屿是封闭岛屿，因为这座岛屿完全被水域包围（即被 1 区域包围）。
```

**示例 2：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/07/sample_4_1610.png)

```
输入：grid = [[0,0,1,0,0],[0,1,0,1,0],[0,1,1,1,0]]
输出：1
```

**示例 3：**

```
输入：grid = [[1,1,1,1,1,1,1],
             [1,0,0,0,0,0,1],
             [1,0,1,1,1,0,1],
             [1,0,1,0,1,0,1],
             [1,0,1,1,1,0,1],
             [1,0,0,0,0,0,1],
             [1,1,1,1,1,1,1]]
输出：2
```

**提示：**

- `1 <= grid.length, grid[0].length <= 100`
- `0 <= grid[i][j] <=1`

**解法一：DFS**

先把与边界连通的 `0` 用 `DFS` 改为 `1`，然后再 `DFS` 遍历 `0` 的个数

```python
class Solution:
    def closedIsland(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        def dfs(row, col):
            if grid[row][col] == 0:
                grid[row][col] = 1
                for r, c in [(row + 1, col), (row - 1, col), (row, col + 1), (row, col - 1)]:
                    if 0 <= r < m and 0 <= c < n and grid[r][c] == 0:
                        dfs(r, c)
        for i in range(n):
            dfs(0, i)
            dfs(m - 1, i)
        for i in range(m):
            dfs(i, 0)
            dfs(i, n - 1)
        ans = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 0:
                    dfs(i, j)
                    ans += 1
        return ans
```

**解法二：并查集**

先把与边界连通的 `0` 用 `DFS` 改为 `1`，然后再 `DFS` 求连通分量的个数

```python
class Solution:
    def closedIsland(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        def dfs(row, col):
            if grid[row][col] == 0:
                grid[row][col] = 1
                for r, c in [(row + 1, col), (row - 1, col), (row, col + 1), (row, col - 1)]:
                    if 0 <= r < m and 0 <= c < n and grid[r][c] == 0:
                        dfs(r, c)
        for i in range(n):
            dfs(0, i)
            dfs(m - 1, i)
        for i in range(m):
            dfs(i, 0)
            dfs(i, n - 1)
        uf = UnionFind(grid)
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 0:
                    idx = i * n + j
                    if j + 1 < n - 1 and grid[i][j + 1] == 0:
                        uf.union(idx, idx + 1)
                    if i + 1 < m - 1 and grid[i + 1][j] == 0:
                        uf.union(idx, idx + n)
        return uf.count

class UnionFind:
    def __init__(self, grid):
        m, n = len(grid), len(grid[0])
        self.ancestor = [i for i in range(m * n)]
        self.count = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 0:
                    self.count += 1

    def find(self, x):
        if x != self.ancestor[x]:
            self.ancestor[x] = self.find(self.ancestor[x])
        return self.ancestor[x]
    
    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX == rootY:
            return
        self.ancestor[rootX] = rootY
        self.count -= 1
```

