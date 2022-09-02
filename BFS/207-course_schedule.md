### [207. 课程表](https://leetcode.cn/problems/course-schedule/)

- 中等

你这个学期必须选修 `numCourses` 门课程，记为 `0` 到 `numCourses - 1` 。

在选修某些课程之前需要一些先修课程。 先修课程按数组 `prerequisites` 给出，其中 `prerequisites[i] = [ai, bi]` ，表示如果要学习课程 `ai` 则 **必须** 先学习课程 `bi` 。

- 例如，先修课程对 `[0, 1]` 表示：想要学习课程 `0` ，你需要先完成课程 `1` 。

请你判断是否可能完成所有课程的学习？如果可以，返回 `true` ；否则，返回 `false` 。

**示例 1：**

```
输入：numCourses = 2, prerequisites = [[1,0]]
输出：true
解释：总共有 2 门课程。学习课程 1 之前，你需要完成课程 0 。这是可能的。
```

**示例 2：**

```
输入：numCourses = 2, prerequisites = [[1,0],[0,1]]
输出：false
解释：总共有 2 门课程。学习课程 1 之前，你需要先完成​课程 0 ；并且学习课程 0 之前，你还应先完成课程 1 。这是不可能的。
```

**提示：**

- `1 <= numCourses <= 10^5`
- `0 <= prerequisites.length <= 5000`

- `prerequisites[i].length == 2`
- `0 <= ai, bi < numCourses`

- `prerequisites[i]` 中的所有课程对 **互不相同**

**解法一：BFS+拓扑排序**

可以把课程表看成**先修课程->课程**的有向图，想要能够上完所有的课，需要该图**存在拓扑排序**

**拓扑排序：**

给定一个包含 `n` 个节点的有向图 `G`，我们给出它的节点编号的一种排列，如果满足：

对于图 `G` 中的任意一条有向边 `(u, v)`，`u` 在排列中都出现在 `v` 的前面。

那么称该排列是图 `G` 的「拓扑排序」。根据上述的定义，我们可以得出两个结论：

如果图 `G` 中存在环（即图 `G` 不是「有向无环图」），那么图 GG 不存在拓扑排序。这是因为假设图中存在环 $x_1, x_2, \cdots, x_n, x_1$，那么 $x_1$ 在排列中必须出现在 $x_n$ 的前面，但 $x_n$  同时也必须出现在 $x_1$ 的前面，因此不存在一个满足要求的排列，也就不存在拓扑排序；

如果图 `G` 是有向无环图，那么它的拓扑排序可能不止一种。举一个最极端的例子，如果图 `G` 值包含 `n` 个节点却没有任何边，那么任意一种编号的排列都可以作为拓扑排序。

有了上述的简单分析，我们就可以将本题建模成一个求拓扑排序的问题了：

我们将每一门课看成一个节点；

如果想要学习课程 `A` 之前必须完成课程 `B`，那么我们从 `B` 到 `A` 连接一条有向边。这样以来，在拓扑排序中，`B` 一定出现在 `A` 的前面。

用 `BFS` 遍历拓扑图，从"最左边"开始遍历，即入度为 0 的节点开始，遍历每个节点的下一个节点，使它们的入度减一，如果该节点入度为 0 的话，就入队

最后看拓扑序列是否包含了所有节点

```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        # 要先找到能学的课程，不存在先导的课程，即入度为 0 的节点
        # 看成有向图，要找到拓扑排序
        # 先构造每个节点的出边和入度
        n = len(prerequisites)
        edges = defaultdict(list)
        inDegree = [0] * numCourses
        for i in range(n):
            edges[prerequisites[i][1]].append(prerequisites[i][0])
            inDegree[prerequisites[i][0]] += 1
        # 把入度为 0 的节点入队
        q = deque([node for node in range(numCourses) if inDegree[node] == 0])
        visited = []
        while q:
            u = q.popleft()
            visited.append(u)
            for v in edges[u]:
                inDegree[v] -= 1
                if inDegree[v] == 0:
                    q.append(v)
        return len(visited) == numCourses
```

