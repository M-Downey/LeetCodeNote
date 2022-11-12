### [64. 最小路径和](https://leetcode.cn/problems/minimum-path-sum/)

- 中等

给定一个包含非负整数的 `*m* x *n*` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg)

```
输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
输出：7
解释：因为路径 1→3→1→1→1 的总和最小。
```

**示例 2：**

```
输入：grid = [[1,2,3],[4,5,6]]
输出：12
```

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 200`
- `0 <= grid[i][j] <= 100`

**解法一：回溯**

会超时

```python
class Solution:
    def __init__(self):
        self.ans = float('inf')

    def minPathSum(self, grid: List[List[int]]) -> int:
        visited = set((0, 0))
        self.dfs([(0, 0)], 0, 0, grid, visited)
        return self.ans
    
    def dfs(self, path, sx, sy, grid, visited):
        m, n = len(grid), len(grid[0])
        if (sx, sy) == (m - 1, n - 1):
            tmp = 0
            # print(path)
            for x, y in path:
                tmp += grid[x][y]
            self.ans = min(tmp, self.ans)
            return
        for x, y in [(sx + 1, sy), (sx, sy + 1)]:
            if 0 <= x < m and 0 <= y < n and (x, y) not in visited:
                path.append((x, y))
                visited.add((x, y))
                self.dfs(path, x, y, grid, visited)
                path.pop()
                visited.remove((x, y))
```

**解法二：DP**

```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        # dp[i][j] 代表从 (0, 0) 到 (i, j) 处的最小花费
        # dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j]
        m, n = len(grid), len(grid[0])
        dp = [[0] * n for _ in range(m)]
        dp[0][0] = grid[0][0]
        for j in range(1, n):
            dp[0][j] = dp[0][j - 1] + grid[0][j]
        for i in range(1, m):
            dp[i][0] = dp[i - 1][0] + grid[i][0]
        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j]
        return dp[m - 1][n - 1] 
```

