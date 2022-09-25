### [1391. 检查网格中是否存在有效路径](https://leetcode.cn/problems/check-if-there-is-a-valid-path-in-a-grid/)

- 中等

给你一个 *m* x *n* 的网格 `grid`。网格里的每个单元都代表一条街道。`grid[i][j]` 的街道可以是：

- **1** 表示连接左单元格和右单元格的街道。
- **2** 表示连接上单元格和下单元格的街道。
- **3** 表示连接左单元格和下单元格的街道。
- **4** 表示连接右单元格和下单元格的街道。
- **5** 表示连接左单元格和上单元格的街道。
- **6** 表示连接右单元格和上单元格的街道。

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/21/main.png)

你最开始从左上角的单元格 `(0,0)` 开始出发，网格中的「有效路径」是指从左上方的单元格 `(0,0)` 开始、一直到右下方的 `(m-1,n-1)` 结束的路径。**该路径必须只沿着街道走**。

**注意：**你 **不能** 变更街道。

如果网格中存在有效的路径，则返回 `true`，否则返回 `false` 。

**示例 1：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/21/e1.png)

```
输入：grid = [[2,4,3],[6,5,2]]
输出：true
解释：如图所示，你可以从 (0, 0) 开始，访问网格中的所有单元格并到达 (m - 1, n - 1) 。
```

**示例 2：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/03/21/e2.png)

```
输入：grid = [[1,2,1],[1,2,1]]
输出：false
解释：如图所示，单元格 (0, 0) 上的街道没有与任何其他单元格上的街道相连，你只会停在 (0, 0) 处。
```

**示例 3：**

```
输入：grid = [[1,1,2]]
输出：false
解释：你会停在 (0, 1)，而且无法到达 (0, 2) 。
```

**示例 4：**

```
输入：grid = [[1,1,1,1,1,1,3]]
输出：true
```

**示例 5：**

```
输入：grid = [[2],[2],[2],[2],[2],[2],[6]]
输出：true
```

**提示：**

- `m == grid.length`
- `n == grid[i].length`

- `1 <= m, n <= 300`
- `1 <= grid[i][j] <= 6`

**解法一：并查集**

依次遍历方格，用并查集维护方格之间的连通性

```python
class Solution:
    def hasValidPath(self, grid: List[List[int]]) -> bool:
        m, n = len(grid), len(grid[0])
        uf = UnionFind(m * n)
        for i in range(m):
            for j in range(n):
                if j + 1 < n:
                    if grid[i][j] == 1:
                        right = grid[i][j + 1]
                        if right in [1, 3, 5]:
                            uf.union(i * n + j, i * n + j + 1)
                    if grid[i][j] == 4:
                        right = grid[i][j + 1]
                        if right in [1, 3, 5]:
                            uf.union(i * n + j, i * n + j + 1)
                    if grid[i][j] == 6:
                        right = grid[i][j + 1]
                        if right in [1, 3, 5]:
                            uf.union(i * n + j, i * n + j + 1)
                if i + 1 < m:
                    if grid[i][j] == 2:
                        down = grid[i + 1][j]
                        if down in [2, 5, 6]:
                            uf.union(i * n + j, (i + 1) * n + j)
                    if grid[i][j] == 3:
                        down = grid[i + 1][j]
                        if down in [2, 5, 6]:
                            uf.union(i * n + j, (i + 1) * n + j)
                    if grid[i][j] == 4:
                        down = grid[i + 1][j]
                        if down in [2, 5, 6]:
                            uf.union(i * n + j, (i + 1) * n + j)
        return uf.isConnected(0, m * n - 1)

class UnionFind:
    def __init__(self, n):
        self.ancestor = [i for i in range(n)]
        self.size = [1] * n

    def find(self, x):
        if x != self.ancestor[x]:
            self.ancestor[x] = self.find(self.ancestor[x])
        return self.ancestor[x]
    
    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX == rootY:
            return
        if rootX < rootY:
            rootX, rootY = rootY, rootX
        self.ancestor[rootY] = rootX
        self.size[rootX] += self.size[rootY]
    
    def isConnected(self, x, y):
        return self.find(x) == self.find(y)
```

