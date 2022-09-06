### [1091. 二进制矩阵中的最短路径](https://leetcode.cn/problems/shortest-path-in-binary-matrix/)

- 中等

给你一个 `n x n` 的二进制矩阵 `grid` 中，返回矩阵中最短 **畅通路径** 的长度。如果不存在这样的路径，返回 `-1` 。

二进制矩阵中的 畅通路径 是一条从 **左上角** 单元格（即，`(0, 0)`）到 右下角 单元格（即，`(n - 1, n - 1)`）的路径，该路径同时满足下述要求：

- 路径途经的所有单元格都的值都是 `0` 。

- 路径中所有相邻的单元格应当在 **8 个方向之一** 上连通（即，相邻两单元之间彼此不同且共享一条边或者一个角）。

**畅通路径的长度** 是该路径途经的单元格总数。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2021/02/18/example1_1.png)

```
输入：grid = [[0,1],[1,0]]
输出：2
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2021/02/18/example2_1.png)

```
输入：grid = [[0,0,0],[1,1,0],[1,1,0]]
输出：4
```

**示例 3：**

```
输入：grid = [[1,0,0],[1,1,0],[1,1,0]]
输出：-1
```

**提示：**

- `n == grid.length`
- `n == grid[i].length`

- `1 <= n <= 100`
- `grid[i][j]` 为 `0` 或 `1`

**解法一：BFS**

本题目是常规 `BFS` ，只不过四方向变成了八方向

```python
class Solution:
    def shortestPathBinaryMatrix(self, grid: List[List[int]]) -> int:
        # 每次把新的一圈入队
        # 直接改矩阵为 1 来标记是否可以走
        if grid[0][0] != 0:
            return -1
        if len(grid) == 1:
            return 1
        q = [(0, 0)]
        dirs = [(-1, -1), (-1, 0), (-1, 1), (0, -1), (0, 1), (1, -1), (1, 0), (1, 1)]
        n = len(grid)
        ans = 1
        while q:
            tmp = []
            for row, col in q:
                for dx, dy in dirs:
                    r = row + dx
                    c = col + dy
                    if 0 <= r < n and 0 <= c < n and grid[r][c] == 0:
                        if r == n - 1 and c == n - 1:
                            return ans + 1
                        grid[r][c] = 1
                        tmp.append((r, c))
            if tmp:
                ans += 1
            q = tmp
        return -1
```

