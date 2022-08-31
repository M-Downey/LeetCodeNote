### [547. 省份数量](https://leetcode.cn/problems/number-of-provinces/)

- 中等

有 `n` 个城市，其中一些彼此相连，另一些没有相连。如果城市 `a` 与城市 `b` 直接相连，且城市 `b` 与城市 `c` 直接相连，那么城市 `a` 与城市 `c` 间接相连。

**省份** 是一组直接或间接相连的城市，组内不含其他没有相连的城市。

给你一个 `n x n` 的矩阵 `isConnected` ，其中 `isConnected[i][j] = 1` 表示第 `i` 个城市和第 `j` 个城市直接相连，而 `isConnected[i][j] = 0` 表示二者不直接相连。

返回矩阵中 **省份** 的数量。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg)

```
输入：isConnected = [[1,1,0],[1,1,0],[0,0,1]]
输出：2
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2020/12/24/graph2.jpg)

```
输入：isConnected = [[1,0,0],[0,1,0],[0,0,1]]
输出：3
```

**提示：**

- `1 <= n <= 200`
- `n == isConnected.length`
- `n == isConnected[i].length`

- `isConnected[i][j]` 为 `1` 或 `0`
- `isConnected[i][i] == 1`

- `isConnected[i][j] == isConnected[j][i]`

**解法一：BFS**

遍历每个没访问过的城市，`BFS` 把该城市周围的城市都访问了，然后 `province` 加一

```python
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        n = len(isConnected)
        visited = set()
        ans = 0
        for i in range(n):
            if i not in visited:
                q = deque([i])
                while q:
                    city = q.popleft()
                    visited.add(city)
                    # 相邻的没访问过的城市入队
                    for j in range(n):
                        if isConnected[city][j] == 1 and j not in visited:
                            q.append(j)
                ans += 1
        return ans
```

**解法二：DFS**

遍历每个没访问过的城市，`DFS` 把该城市周围的城市都访问了，然后 `province` 加一

```python
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        # DFS 遍历每个城市的相邻城市
        n = len(isConnected)
        ans = 0
        visited = set()
        for i in range(n):
            if i not in visited:
                self.dfs(isConnected, i, visited)
                ans += 1
        return ans

    def dfs(self, isConnected, city, visited):
        visited.add(city)
        for i in range(len(isConnected)):
            if isConnected[city][i] == 1 and i not in visited:
                self.dfs(isConnected, i, visited)
```

