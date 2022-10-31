### [1971. 寻找图中是否存在路径](https://leetcode.cn/problems/find-if-path-exists-in-graph/)

- 简单

有一个具有 `n` 个顶点的 **双向** 图，其中每个顶点标记从 `0` 到 `n - 1`（包含 `0` 和 `n - 1`）。图中的边用一个二维整数数组 `edges` 表示，其中 `edges[i] = [ui, vi]` 表示顶点 `ui` 和顶点 `vi` 之间的双向边。 每个顶点对由 **最多一条** 边连接，并且没有顶点存在与自身相连的边。

请你确定是否存在从顶点 `source` 开始，到顶点 `destination` 结束的 **有效路径** 。

给你数组 `edges` 和整数 `n`、`source` 和 `destination`，如果从 `source` 到 `destination` 存在 **有效路径** ，则返回 `true`，否则返回 `false` 。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex1.png)

```
输入：n = 3, edges = [[0,1],[1,2],[2,0]], source = 0, destination = 2
输出：true
解释：存在由顶点 0 到顶点 2 的路径:
- 0 → 1 → 2 
- 0 → 2
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex2.png)

```
输入：n = 6, edges = [[0,1],[0,2],[3,5],[5,4],[4,3]], source = 0, destination = 5
输出：false
解释：不存在由顶点 0 到顶点 5 的路径.
```

**提示：**

- `1 <= n <= 2 * 10^5`
- `0 <= edges.length <= 2 * 10^5`
- `edges[i].length == 2`
- `0 <= ui, vi <= n - 1`
- `ui != vi`
- `0 <= source, destination <= n - 1`
- 不存在重复边
- 不存在指向顶点自身的边

**解法一：DFS**

```python
class Solution:
    def validPath(self, n: int, edges: List[List[int]], source: int, destination: int) -> bool:
        def dfs(src):
            visited.add(src)
            for y in node_edges[src]:
                if y not in visited:
                    dfs(y)
        node_edges = defaultdict(list)
        for x, y in edges:
            node_edges[x].append(y)
            node_edges[y].append(x)
        visited = set()
        dfs(source)
        return destination in visited
```

**解法二：并查集**

```python
class Solution:
    def validPath(self, n: int, edges: List[List[int]], source: int, destination: int) -> bool:
        uf = UnionFind(n)
        for x, y in edges:
            uf.union(x, y)
        return uf.connected(source, destination)
    
class UnionFind:
    def __init__(self, n):
        self.ancetor = [i for i in range(n)]
        self.size = [1] * n
    
    def find(self, x):
        if x != self.ancetor[x]:
            self.ancetor[x] = self.find(self.ancetor[x])
        return self.ancetor[x]
    
    def union(self, x, y):
        rootX, rootY = self.find(x), self.find(y)
        if rootX == rootY:
            return
        if self.size[rootX] < self.size[rootY]:
            rootX, rootY = rootY, rootX
        self.ancetor[rootY] = rootX
        self.size[rootX] += self.size[rootY]
    
    def connected(self, x, y):
        return self.find(x) == self.find(y)
```

