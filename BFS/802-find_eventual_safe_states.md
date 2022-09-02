### [802. 找到最终的安全状态](https://leetcode.cn/problems/find-eventual-safe-states/)

- 中等

有一个有 `n` 个节点的有向图，节点按 `0` 到 `n - 1` 编号。图由一个 **索引从 0 开始** 的 2D 整数数组 `graph`表示， `graph[i]`是与节点 `i` 相邻的节点的整数数组，这意味着从节点 `i` 到 `graph[i]`中的每个节点都有一条边。

如果一个节点没有连出的有向边，则它是 **终端节点** 。如果没有出边，则节点为终端节点。如果从该节点开始的所有可能路径都通向 **终端节点** ，则该节点为 **安全节点** 。

返回一个由图中所有 **安全节点** 组成的数组作为答案。答案数组中的元素应当按 **升序** 排列。

**示例 1：**

 ![Illustration of graph](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/03/17/picture1.png)

```
输入：graph = [[1,2],[2,3],[5],[0],[5],[],[]]
输出：[2,4,5,6]
解释：示意图如上。
节点 5 和节点 6 是终端节点，因为它们都没有出边。
从节点 2、4、5 和 6 开始的所有路径都指向节点 5 或 6 。
```

**示例 2：**

```
输入：graph = [[1,2,3,4],[1,2],[3,4],[0,4],[]]
输出：[4]
解释:
只有节点 4 是终端节点，从节点 4 开始的所有路径都通向节点 4 。
```

**提示：**

- `n == graph.length`
- `1 <= n <= 10^4`

- `0 <= graph[i].length <= n`
- `0 <= graph[i][j] <= n - 1`

- `graph[i]` 按严格递增顺序排列。
- 图中可能包含自环。
- 图中边的数目在范围 `[1, 4 * 10^4]` 内。

**解法一：拓扑排序+反图**

根据题意，**若一个节点没有出边，则该节点是安全的；若一个节点出边相连的点都是安全的，则该节点也是安全的。**

**若一个节点通往的所有路径都到达终端节点，则在反图中，到达该节点的所有路径都来自终端节点，即可以从终端遍历到所有到达该节点**

根据这一性质，我们可以将图中所有边反向，得到一个反图，然后在反图上运行拓扑排序。

具体来说，首先得到反图 $\textit{rg}$ 及其入度数组 $\textit{inDeg}$。将所有入度为 `0` 的点加入队列，然后不断取出队首元素，将其出边相连的点的入度减一，若该点入度减一后为 `0`，则将该点加入队列，如此循环直至队列为空。循环结束后，所有入度为 `0` 的节点均为安全的。我们遍历入度数组，并将入度为 `0` 的点加入答案列表。

```python
class Solution:
    def eventualSafeNodes(self, graph: List[List[int]]) -> List[int]:
        # 一个节点的所有终点节点都是终端的话，那么在反图中，到该节点的所有路径都从终端出发
        # 所以从终端 BFS 遇到该节点，入度就减一，直到入度为0
        reverse_graph = defaultdict(list)
        n = len(graph)
        inDegree = [0] * n
        for i in range(n):
            u, vs = i, graph[i]
            for v in vs:
                reverse_graph[v].append(u)
                inDegree[u] += 1
        print(reverse_graph)
        q = deque([node for node in range(n) if inDegree[node] == 0])
        while q:
            u = q.popleft()
            for v in reverse_graph[u]:
                inDegree[v] -= 1
                if inDegree[v] == 0:
                    q.append(v)
        return [u for u in range(n) if inDegree[u] == 0]
```

