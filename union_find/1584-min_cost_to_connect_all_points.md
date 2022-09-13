### [1584. 连接所有点的最小费用](https://leetcode.cn/problems/min-cost-to-connect-all-points/)

- 中等

给你一个`points` 数组，表示 2D 平面上的一些点，其中 `points[i] = [xi, yi]` 。

连接点 `[xi, yi]` 和点 `[xj, yj]` 的费用为它们之间的 **曼哈顿距离** ：`|xi - xj| + |yi - yj|` ，其中 `|val|` 表示 `val` 的绝对值。

请你返回将所有点连接的最小总费用。只有任意两点之间 **有且仅有** 一条简单路径时，才认为所有点都已连接。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2020/08/26/d.png)

 ![img](https://assets.leetcode.com/uploads/2020/08/26/c.png)

```
输入：points = [[0,0],[2,2],[3,10],[5,2],[7,0]]
输出：20

我们可以按照上图所示连接所有点得到最小总费用，总费用为 20 。
注意到任意两个点之间只有唯一一条路径互相到达。
```

**示例 2：**

```
输入：points = [[3,12],[-2,5],[-4,1]]
输出：18
```

**示例 3：**

```
输入：points = [[0,0],[1,1],[1,0],[-1,1]]
输出：4
```

**示例 4：**

```
输入：points = [[-1000000,-1000000],[1000000,1000000]]
输出：4000000
```

**示例 5：**

```
输入：points = [[0,0]]
输出：0
```

**提示：**

- `1 <= points.length <= 1000`
- `-10^6 <= xi, yi <= 10^6`
- 所有点 `(xi, yi)` 两两不同。

**解法一：Kruskal最小生成树**

```python
class Solution:
    def minCostConnectPoints(self, points: List[List[int]]) -> int:
        # 把点坐标标号即可
        n = len(points)
        uf = UnionFind(n)
        edges = []
        dist = lambda x, y : abs(points[x][0] - points[y][0]) + abs(points[x][1] - points[y][1])
        for i in range(n):
            for j in range(i + 1, n):
                edges.append((i, j, dist(i, j)))
        edges.sort(key=lambda x: x[2])
        ans = 0
        for x, y, weight in edges:
            if uf.union(x, y):
                ans += weight
            if uf.count == 1:
                break
        return ans

class UnionFind:
    def __init__(self, n):
        self.ancestor = [i for i in range(n)]
        self.size = [1] * n
        self.count = n
    
    def find(self, x):
        if x != self.ancestor[x]:
            self.ancestor[x] = self.find(self.ancestor[x])
        return self.ancestor[x]
    
    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX == rootY:
            return False
        if self.size[rootX] < self.size[rootY]:
            rootX, rootY = rootY, rootX
        self.ancestor[rootY] = rootX
        self.size[rootX] += self.size[rootY]
        self.count -= 1
        return True
```

