### [959. 由斜杠划分区域](https://leetcode.cn/problems/regions-cut-by-slashes/)

- 中等

在由 `1 x 1` 方格组成的 `n x n` 网格 `grid` 中，每个 `1 x 1` 方块由 `'/'`、`'\'` 或空格构成。这些字符会将方块划分为一些共边的区域。

给定网格 `grid` 表示为一个字符串数组，返回 *区域的数量* 。

请注意，反斜杠字符是转义的，因此 `'\'` 用 `'\\'` 表示。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2018/12/15/1.png)

```
输入：grid = [" /","/ "]
输出：2
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2018/12/15/2.png)

```
输入：grid = [" /","  "]
输出：1
```

**示例 3：**

 ![img](https://assets.leetcode.com/uploads/2018/12/15/4.png)

```
输入：grid = ["/\\","\\/"]
输出：5
解释：回想一下，因为 \ 字符是转义的，所以 "/\\" 表示 /\，而 "\\/" 表示 \/。
```

**提示：**

- `n == grid.length == grid[i].length`
- `1 <= n <= 30`
- `grid[i][j]` 是 `'/'`、`'\'`、或 `' '`

**解法一：并查集**

把每一个小单元格分为4个小三角区域，然后用并查集

单元格内合并：

- 如果是' '，那么需要把0 1 2 3合并
- 如果是'/'，那么需要把0 3合并，1 2合并
- 如果是'\\'，那么需要把0 1合并，2 3合并

单元格间合并：

由于单元格连接的地方是天然连接的，所以在当前单元格右边和下面连接处，要把1 3合并，2 0合并

并查集中添加变量 `count` 用于记录连通分量的个数，最后返回 `count` 即可

```python
class Solution:
    def regionsBySlashes(self, grid: List[str]) -> int:
        # 每个小方格分成4个小三角形区域
        # 如果是空格就合并 0 1 2 3
        # 如果是 / 就合并 0 3 | 1 2
        # 如果是 \ 就合并 0 1 | 2 3
        # 小方格之间合并右边的1 3和下边的2 0
        n = len(grid)
        uf = UnionFind(n * n * 4)
        for i in range(n):
            for j in range(n):
                # 每个小方格中的 0 号三角的索引
                index = (i * n + j) * 4
                if grid[i][j] == ' ':
                    uf.union(index, index + 1)
                    uf.union(index + 1, index + 2)
                    uf.union(index + 2, index + 3)
                elif grid[i][j] == '/':
                    uf.union(index, index + 3)
                    uf.union(index + 1, index + 2)
                elif grid[i][j] == '\\':
                    uf.union(index, index + 1)
                    uf.union(index + 2, index + 3)
                if j + 1 < n:
                    uf.union(index + 1, index + 7)
                if i + 1 < n:
                    uf.union(index + 2, index + 4 * n)
        return uf.count

class UnionFind:
    def __init__(self, n):
        self.count = n
        self.parent = [i for i in range(n)]
    
    def find(self, x):
        if x != self.parent[x]:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX == rootY:
            return
        self.parent[rootX] = rootY
        self.count -= 1
```

