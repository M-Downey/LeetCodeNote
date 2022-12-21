### [1697. 检查边长度限制的路径是否存在](https://leetcode.cn/problems/checking-existence-of-edge-length-limited-paths/)

- 困难

给你一个 `n` 个点组成的无向图边集 `edgeList` ，其中 `edgeList[i] = [ui, vi, disi]` 表示点 `ui` 和点 `vi` 之间有一条长度为 `disi` 的边。请注意，两个点之间可能有 **超过一条边** 。

给你一个查询数组`queries` ，其中 `queries[j] = [pj, qj, limitj]` ，你的任务是对于每个查询 `queries[j]` ，判断是否存在从 `pj` 到 `qj` 的路径，且这条路径上的每一条边都 **严格小于** `limitj` 。

请你返回一个 **布尔数组** `answer` ，其中 `answer.length == queries.length` ，当 `queries[j]` 的查询结果为 `true` 时， `answer` 第 `j` 个值为 `true` ，否则为 `false` 。

**示例 1：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/12/19/h.png)

```
输入：n = 3, edgeList = [[0,1,2],[1,2,4],[2,0,8],[1,0,16]], queries = [[0,1,2],[0,2,5]]
输出：[false,true]
解释：上图为给定的输入数据。注意到 0 和 1 之间有两条重边，分别为 2 和 16 。
对于第一个查询，0 和 1 之间没有小于 2 的边，所以我们返回 false 。
对于第二个查询，有一条路径（0 -> 1 -> 2）两条边都小于 5 ，所以这个查询我们返回 true 。
```

**示例 2：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/12/19/q.png)

```
输入：n = 5, edgeList = [[0,1,10],[1,2,5],[2,3,9],[3,4,13]], queries = [[0,4,14],[1,4,13]]
输出：[true,false]
解释：上图为给定数据。
```

**提示：**

- `2 <= n <= 10^5`
- `1 <= edgeList.length, queries.length <= 10^5`
- `edgeList[i].length == 3`
- `queries[j].length == 3`
- `0 <= ui, vi, pj, qj <= n - 1`
- `ui != vi`
- `pj != qj`
- `1 <= disi, limitj <= 10^9`
- 两个点之间可能有 **多条** 边。

**解法一：并查集**

给定一个查询时，我们可以遍历 `edgeList` 中的所有边，依次将长度小于 `limit` 的边加入到并查集中，然后使用并查集查询 `p` 和 `q` 是否属于同一个集合。如果 `p` 和 `q` 属于同一个集合，则说明存在从 `p` 到 `q` 的路径，且这条路径上的每一条边的长度都严格小于 `limit` ，查询返回 `true` ，否则查询返回 `false`。

如果 `queries` 的 `limit` 是非递减的，显然上一次查询的并查集里的边都是满足当前查询的 `limit` 要求的，我们只需要将剩余的长度小于 `limit` 的边加入并查集中即可。基于此，我们首先将 `edgeList` 按边长度从小到大进行排序，然后将 `querics` 按 `limit` 从小到大进行排序，使用 `k` 指向上一次查询中不满足 `limit` 要求的长度最小的边，显然初始时 `k=0`。

我们依次遍历 `queries` :如果 `k` 指向的边的长度小于对应查询的 `limit` ，则将该边加入并查集中，然后将 `k` 加 `1` ，直到 `k` 指向的边不满足要求;最后根据并查集查询对应的 `p` 和 `q` 是否属于同一集合来保存查询的结果。

```python
class Solution:
    def distanceLimitedPathsExist(self, n: int, edgeList: List[List[int]], queries: List[List[int]]) -> List[bool]:
        # 对于一个查询，考虑用并查集判断是否存在一条边都小于 limit 的路径
        # 按边权值从小到大的顺序合并节点，把所有小于 limit 的边都合并在一起，最后判断 p q 是否相连
        # 把所有 query 按 limit 从小到大排序，用 k 指向当前添加的第 k 条边
        # 因为对于更大的 limit 来说，前面的边也都是要合并的，所以可以减少运算
        # 遍历 query ，对每个 query 来说合并小于其 limit 的边
        uf = UnionFind(n)
        edgeList.sort(key=lambda x: x[2])
        
        q_len = len(queries)
        ans = [False] * q_len
        k = 0
        # 注意 for 这个操作，因为需要标记原始位置，所以要带着索引一起排序
        for i, (p, q, limit) in sorted(enumerate(queries), key=lambda x: x[1][2]):
            while k < len(edgeList) and edgeList[k][2] < limit:
                uf.union(edgeList[k][0], edgeList[k][1])
                k += 1
            if uf.isConnected(p, q):
                ans[i] = True
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
        rootX, rootY = self.find(x), self.find(y)
        if rootX == rootY:
            return
        if self.size[rootX] < self.size[rootY]:
            rootX, rootY = rootY, rootX
        self.size[rootX] += self.size[rootY]
        self.ancestor[rootY] = rootX
    
    def isConnected(self, x, y):
        return self.find(x) == self.find(y)
```

