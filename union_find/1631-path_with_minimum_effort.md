### [1631. 最小体力消耗路径](https://leetcode.cn/problems/path-with-minimum-effort/)

- 中等

你准备参加一场远足活动。给你一个二维 `rows x columns` 的地图 `heights` ，其中 `heights[row][col]` 表示格子 `(row, col)` 的高度。一开始你在最左上角的格子 `(0, 0)` ，且你希望去最右下角的格子 `(rows-1, columns-1)` （注意下标从 **0** 开始编号）。你每次可以往 **上**，**下**，**左**，**右** 四个方向之一移动，你想要找到耗费 **体力** 最小的一条路径。

一条路径耗费的 **体力值** 是路径上相邻格子之间 **高度差绝对值** 的 **最大值** 决定的。

请你返回从左上角走到右下角的最小 **体力消耗值** 。

**示例 1：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/ex1.png)

```
输入：heights = [[1,2,2],[3,8,2],[5,3,5]]
输出：2
解释：路径 [1,3,5,3,5] 连续格子的差值绝对值最大为 2 。
这条路径比路径 [1,2,2,2,5] 更优，因为另一条路径差值最大值为 3 。
```

**示例 2：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/ex2.png)

```
输入：heights = [[1,2,3],[3,8,4],[5,3,5]]
输出：1
解释：路径 [1,2,3,4,5] 的相邻格子差值绝对值最大为 1 ，比路径 [1,3,5,3,5] 更优。
```

**示例 3：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/10/25/ex3.png)

```
输入：heights = [[1,2,1,1,1],[1,2,1,2,1],[1,2,1,2,1],[1,2,1,2,1],[1,1,1,2,1]]
输出：0
解释：上图所示路径不需要消耗任何体力。
```

**提示：**

- `rows == heights.length`
- `columns == heights[i].length`

- `1 <= rows, columns <= 100`
- `1 <= heights[i][j] <= 10^6`

**解法一：类Kruskal**

把每个点用右和下方向的边连起来并存储节点和权值，然后按权值升序排列

并查集从低权值开始合并，直到 `(0, 0)` 和 `(m - 1, n - 1)` 连通

当前的 `weight` 就是最小体力消耗值

```python
class Solution:
    def minimumEffortPath(self, heights: List[List[int]]) -> int:
        m, n = len(heights), len(heights[0])
        uf = UnionFind(m * n)
        edges = []
        for i in range(m):
            for j in range(n):
                idx = i * n + j
                if i < m - 1:
                    edges.append((idx, idx + n, abs(heights[i][j] - heights[i + 1][j])))
                if j < n - 1:
                    edges.append((idx, idx + 1, abs(heights[i][j] - heights[i][j + 1])))
        edges.sort(key=lambda x: x[2])
        ans = 0
        for x, y, weight in edges:
            uf.union(x, y)
            if uf.connected(0, m * n - 1):
                ans = weight
                break
        return ans

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
        if self.size[rootX] < self.size[rootY]:
            rootX, rootY = rootY, rootX
        self.ancestor[rootY] = rootX
        self.size[rootX] += self.size[rootY]

    def connected(self, x, y):
        return self.find(x) == self.find(y)
```

