### [934. 最短的桥](https://leetcode.cn/problems/shortest-bridge/)

- 中等

给你一个大小为 `n x n` 的二元矩阵 `grid` ，其中 `1` 表示陆地，`0` 表示水域。

**岛** 是由四面相连的 `1` 形成的一个最大组，即不会与非组内的任何其他 `1` 相连。`grid` 中 **恰好存在两座岛** 。

你可以将任意数量的 `0` 变为 `1` ，以使两座岛连接起来，变成 **一座岛** 。

返回必须翻转的 `0` 的最小数目。

**示例 1：**

```
输入：grid = [[0,1],[1,0]]
输出：1
```

**示例 2：**

```
输入：grid = [[0,1,0],[0,0,0],[0,0,1]]
输出：2
```

**示例 3：**

```
输入：grid = [[1,1,1,1,1],[1,0,0,0,1],[1,0,1,0,1],[1,0,0,0,1],[1,1,1,1,1]]
输出：1
```

**提示：**

- `n == grid.length == grid[i].length`
- `2 <= n <= 100`

- `grid[i][j]` 为 `0` 或 `1`
- `grid` 中恰有两个岛

**解法一：DFS+BFS**

先 `DFS` 找到一个岛，然后把岛上坐标全部入队，然后 `BFS` 遍历岛的周围，每遍历一圈， 桥的长度加一，直到遍历到第二座岛，返回桥长

```python
class Solution:
    def shortestBridge(self, grid: List[List[int]]) -> int:
        # 需要一个岛的边界，周围存在零的
        # 找一个岛
        def dfs(grid, row, col):
            queue.append((row, col))
            visited[row][col] = True
            n = len(grid)
            for r, c in [(row + 1, col), (row - 1, col), (row, col + 1), (row, col - 1)]:
                if 0 <= r < n and 0 <= c < n and grid[r][c] == 1 and visited[r][c] == False:
                    dfs(grid, r, c)
        n = len(grid)
        queue = deque()
        visited = [[False for _ in range(n)] for _ in range(n)]
        # print(visited)
        done = 0
        for i in range(n):
            for j in range(n):
                if grid[i][j] == 1:
                    dfs(grid, i, j)
                    done = 1
                if done == 1:
                    break
            if done == 1:
                break
        # print(visited)
        # print(queue)
        ans = 0
        while queue:
            tmp = []
            # print(queue)
            for i, j in queue:
                for r, c in [(i + 1, j), (i - 1, j), (i, j + 1), (i, j - 1)]:
                    if 0 <= r < n and 0 <= c < n and visited[r][c] == False:
                        if grid[r][c] == 1:
                            return ans
                        else:
                            visited[r][c] = True
                            tmp.append((r, c))
            if tmp:
                ans += 1
            queue = tmp
        return ans
```

