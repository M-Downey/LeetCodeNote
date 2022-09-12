### [1267. 统计参与通信的服务器](https://leetcode.cn/problems/count-servers-that-communicate/)

- 中等

这里有一幅服务器分布图，服务器的位置标识在 `m * n` 的整数矩阵网格 `grid` 中，1 表示单元格上有服务器，0 表示没有。

如果两台服务器位于同一行或者同一列，我们就认为它们之间可以进行通信。

请你统计并返回能够与至少一台其他服务器进行通信的服务器的数量。

**示例 1：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/24/untitled-diagram-6.jpg)

```
输入：grid = [[1,0],[0,1]]
输出：0
解释：没有一台服务器能与其他服务器进行通信。
```

**示例 2：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/24/untitled-diagram-4-1.jpg)

```
输入：grid = [[1,0],[1,1]]
输出：3
解释：所有这些服务器都至少可以与一台别的服务器进行通信。
```

**示例 3：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/24/untitled-diagram-1-3.jpg)

```
输入：grid = [[1,1,0,0],[0,0,1,0],[0,0,1,0],[0,0,0,1]]
输出：4
解释：第一行的两台服务器互相通信，第三列的两台服务器互相通信，但右下角的服务器无法与其他服务器通信。
```

**提示：**

- `m == grid.length`
- `n == grid[i].length`

- `1 <= m <= 250`
- `1 <= n <= 250`

- `grid[i][j] == 0 or 1`

**解法一：计数**

先遍历一遍，记录每行服务器个数和每列的服务器个数

再遍历一遍，如果这个位置是服务器，且所在行，列有别的服务器，则 `ans++` 

```python
class Solution:
    def countServers(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        row, col = [0] * m, [0] * n
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    row[i] += 1
                    col[j] += 1
        ans = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1 and (row[i] > 1 or col[j] > 1):
                    ans += 1
        return ans
```

**解法二：并查集（有问题）**

```python
class Solution:
    def countServers(self, grid: List[List[int]]) -> int:
        uf = UnionFind(grid)
        m, n = len(grid), len(grid[0])
        # 遍历每台服务器下边和右边的服务器，进行合并
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    for k in range(n):
                        if grid[i][k] == 1:
                            uf.union(i * n + j, i * n + k)
                    for k in range(m):
                        if grid[k][j] == 1:
                            uf.union(i * n + j, k * n + j)
        # print(uf.ancestor)
        # print(uf.size)
        ans = 0
        visited = set()
        # 遍历每台服务器所处的连通分量，加上该连通分量的服务器个数（大于1才加）
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    x = uf.ancestor[i * n + j]
                    # 就 1 个服务器，不计算
                    if uf.size[x] == 1:
                        break
                    if x not in visited:
                        visited.add(x)
                        ans += uf.size[x]
        return ans

class UnionFind:
    def __init__(self, grid):
        m, n = len(grid), len(grid[0])
        self.ancestor = [i for i in range(m * n)]
        self.size = [1 for _ in range(m * n)]
    
    def find(self, x):
        if x != self.ancestor[x]:
            self.ancestor[x] = self.find(self.ancestor[x])
        return self.ancestor[x]
    
    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX == rootY:
            return
        if self.size[rootX] < self.size[rootY]:
            rootX, rootY = rootY, rootX
        self.ancestor[rootY] = rootX
        self.size[rootX] += self.size[rootY]
```

