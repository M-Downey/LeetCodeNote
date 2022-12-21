### [1970. 你能穿过矩阵的最后一天](https://leetcode.cn/problems/last-day-where-you-can-still-cross/)

- 困难

给你一个下标从 **1** 开始的二进制矩阵，其中 `0` 表示陆地，`1` 表示水域。同时给你 `row` 和 `col` 分别表示矩阵中行和列的数目。

一开始在第 `0` 天，**整个** 矩阵都是 **陆地** 。但每一天都会有一块新陆地被 **水** 淹没变成水域。给你一个下标从 **1** 开始的二维数组 `cells` ，其中 `cells[i] = [ri, ci]` 表示在第 `i` 天，第 `ri` 行 `ci` 列（下标都是从 **1** 开始）的陆地会变成 **水域** （也就是 `0` 变成 `1` ）。

你想知道从矩阵最 **上面** 一行走到最 **下面** 一行，且只经过陆地格子的 **最后一天** 是哪一天。你可以从最上面一行的 **任意** 格子出发，到达最下面一行的 **任意** 格子。你只能沿着 **四个** 基本方向移动（也就是上下左右）。

请返回只经过陆地格子能从最 **上面** 一行走到最 **下面** 一行的 **最后一天** 。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2021/07/27/1.png)

```
输入：row = 2, col = 2, cells = [[1,1],[2,1],[1,2],[2,2]]
输出：2
解释：上图描述了矩阵从第 0 天开始是如何变化的。
可以从最上面一行到最下面一行的最后一天是第 2 天。
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2021/07/27/2.png)

```
输入：row = 2, col = 2, cells = [[1,1],[1,2],[2,1],[2,2]]
输出：1
解释：上图描述了矩阵从第 0 天开始是如何变化的。
可以从最上面一行到最下面一行的最后一天是第 1 天。
```

**示例 3：**

 ![img](https://assets.leetcode.com/uploads/2021/07/27/3.png)

```
输入：row = 3, col = 3, cells = [[1,2],[2,1],[3,3],[2,2],[1,1],[1,3],[2,3],[3,2],[3,1]]
输出：3
解释：上图描述了矩阵从第 0 天开始是如何变化的。
可以从最上面一行到最下面一行的最后一天是第 3 天。
```

**提示：**

- `2 <= row, col <= 2 * 10^4`
- `4 <= row * col <= 2 * 10^4`
- `cells.length == row * col`
- `1 <= ri <= row`
- `1 <= ci <= col`
- `cells` 中的所有格子坐标都是 **唯一** 的。

**解法一：并查集**

- 反向考虑：从全是水开始，每次置一个陆地格子，并把它与周围的陆地格子连起来
- 超级节点：要判断第一行和最后一行的连通性，所以额外添加两个节点分别对应第一行和最后一行，每次添加的节点，如果是第一行或最后一行，就要与之合并

```python
class Solution:
    def latestDayToCross(self, row: int, col: int, cells: List[List[int]]) -> int:
        # 从全水开始变成陆地，每次给一个格子变成陆地，并把它与周围的陆地相连
        # 加入 2 个超级节点，一个是第一行，一个是最后一行，如果当前格子是第一行或最后一行
        # 就要与超级节点相连
        # 最后判断超级节点是否连通
        grid = [[1] * col for _ in range(row)]
        # cells.reverse()
        n = row * col
        # n 和 n + 1 分别代表第一行和最后一行
        uf = UnionFind(n + 2)
        for i in range(n - 1, -1, -1):
            x, y = cells[i][0] - 1, cells[i][1] - 1
            idx = x * col + y
            grid[x][y] = 0
            for nx, ny in [(x + 1, y), (x - 1, y), (x, y + 1), (x, y - 1)]:
                if 0 <= nx < row and 0 <= ny < col and grid[nx][ny] == 0:
                    uf.union(idx, nx * col + ny)
            if x == 0:
                uf.union(idx, n)
            elif x == row - 1:
                uf.union(idx, n + 1)
            # print(uf.ancestor)
            if uf.isConnected(n, n + 1):
                return i
        return 0

class UnionFind:
    def __init__(self, n):
        self.ancestor = [i for i in range(n)]
        self.size = [1] * n

    def find(self, x):
        if x != self.ancestor[x]:
            self.ancestor[x] = self.find(self.ancestor[x])
        return self.ancestor[x]
    
    def union(self, x, y):
        # !写错了，要用find
        rootX, rootY = self.find(x), self.find(y)
        if rootX == rootY:
            return
        if self.size[rootX] < self.size[rootY]:
            rootX, rootY = rootY, rootX
        self.ancestor[rootY] = rootX
        self.size[rootX] += self.size[rootY]
        
    def isConnected(self, x, y):
        return self.find(x) == self.find(y)
```

