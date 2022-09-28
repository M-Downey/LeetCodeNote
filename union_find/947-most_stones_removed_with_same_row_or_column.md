### [947. 移除最多的同行或同列石头](https://leetcode.cn/problems/most-stones-removed-with-same-row-or-column/)

- 中等

`n` 块石头放置在二维平面中的一些整数坐标点上。每个坐标点上最多只能有一块石头。

如果一块石头的 **同行或者同列** 上有其他石头存在，那么就可以移除这块石头。

给你一个长度为 `n` 的数组 `stones` ，其中 `stones[i] = [xi, yi]` 表示第 `i` 块石头的位置，返回 **可以移除的石子** 的最大数量。

**示例 1：**

```
输入：stones = [[0,0],[0,1],[1,0],[1,2],[2,1],[2,2]]
输出：5
解释：一种移除 5 块石头的方法如下所示：
1. 移除石头 [2,2] ，因为它和 [2,1] 同行。
2. 移除石头 [2,1] ，因为它和 [0,1] 同列。
3. 移除石头 [1,2] ，因为它和 [1,0] 同行。
4. 移除石头 [1,0] ，因为它和 [0,0] 同列。
5. 移除石头 [0,1] ，因为它和 [0,0] 同行。
石头 [0,0] 不能移除，因为它没有与另一块石头同行/列。
```

**示例 2：**

```
输入：stones = [[0,0],[0,2],[1,1],[2,0],[2,2]]
输出：3
解释：一种移除 3 块石头的方法如下所示：
1. 移除石头 [2,2] ，因为它和 [2,0] 同行。
2. 移除石头 [2,0] ，因为它和 [0,0] 同列。
3. 移除石头 [0,2] ，因为它和 [0,0] 同行。
石头 [0,0] 和 [1,1] 不能移除，因为它们没有与另一块石头同行/列。
```

**示例 3：**

```
输入：stones = [[0,0]]
输出：0
解释：[0,0] 是平面上唯一一块石头，所以不可以移除它。
```

**提示：**

- `1 <= stones.length <= 1000`
- `0 <= xi, yi <= 10^4`
- 不会有两块石头放在同一个坐标点上

**解法一：并查集**

本题就是要求联通分量的个数，每个连通分量都可以移除至一个石头，所以可以移除的最大石头数就是：

石头数 - 连通分量数

横坐标或纵坐标相同的石头要合并在一起，把横坐标映射到 `[10001, 20001]` 中，然后维护连通性即可

注意，初始的连通分量的个数是**横纵坐标集合的长度**

```python
class Solution:
    def removeStones(self, stones: List[List[int]]) -> int:
        n = len(stones)
        uset = set()
        for x, y in stones:
            uset.add(x + 10001)
            uset.add(y)
        num = len(uset)
        uf = UnionFind(20001, num)
        for x, y in stones:
            uf.union(x + 10001, y)
        return n - uf.count

class UnionFind:
    def __init__(self, n, num):
        self.ancestor = [i for i in range(n)]
        self.size = [1] * n
        self.count = num
    
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
        self.ancestor[rootY] = rootX
        self.size[rootX] += self.size[rootY]
        self.count -= 1
```

**解法二：DFS**

建图，每个石头和同行同列的石头建图，然后 `DFS` 遍历每个石头周围的石头，计算连通分量的个数

```python
class Solution:
    def removeStones(self, stones: List[List[int]]) -> int:
        edges = collections.defaultdict(list)
        for i, (x, y) in enumerate(stones):
            for j, (u, v) in enumerate(stones):
                if u == x or v == y:
                    edges[i].append(j)
        def dfs(x):
            visited.add(x)
            for y in edges[x]:
                if y not in visited:
                    dfs(y)
        visited = set()
        n = len(stones)
        count = 0
        for i in range(n):
            if i not in visited:
                dfs(i)
                count += 1
        return n - count
```

