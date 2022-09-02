### [797. 所有可能的路径](https://leetcode.cn/problems/all-paths-from-source-to-target/)

- 中等

给你一个有 `n` 个节点的 **有向无环图（DAG）**，请你找出所有从节点 `0` 到节点 `n-1` 的路径并输出（**不要求按特定顺序**）

 `graph[i]` 是一个从节点 `i` 可以访问的所有节点的列表（即从节点 `i` 到节点 `graph[i][j]`存在一条有向边）。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2020/09/28/all_1.jpg)

```
输入：graph = [[1,2],[3],[3],[]]
输出：[[0,1,3],[0,2,3]]
解释：有两条路径 0 -> 1 -> 3 和 0 -> 2 -> 3
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2020/09/28/all_2.jpg)

```
输入：graph = [[4,3,1],[3,2,4],[3],[4],[]]
输出：[[0,4],[0,3,4],[0,1,3,4],[0,1,2,3,4],[0,1,4]]
```

**提示：**

- `n == graph.length`
- `2 <= n <= 15`

- `0 <= graph[i][j] < n`
- `graph[i][j] != i`（即不存在自环）

- `graph[i]` 中的所有元素 **互不相同**
- 保证输入为 **有向无环图（DAG）**

**解法一：回溯**

终点：到达节点 `n-1`

循环：每个节点的邻边

```python
class Solution:
    def __init__(self):
        self.ans = []
        self.path = [0]

    def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
        self.backtracking(graph, 0)
        return self.ans

    def backtracking(self, graph, u):
        # print(u)
        # print(self.path)
        if u == len(graph) - 1:
            # print('yes')
            self.ans.append(self.path[:])
            return
        for v in graph[u]:
            self.path.append(v)
            self.backtracking(graph, v)
            self.path.pop()
```

