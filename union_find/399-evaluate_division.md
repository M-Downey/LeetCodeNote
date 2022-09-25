### [399. 除法求值](https://leetcode.cn/problems/evaluate-division/)

- 中等

给你一个变量对数组 `equations` 和一个实数值数组 `values` 作为已知条件，其中 `equations[i] = [Ai, Bi]` 和 `values[i]` 共同表示等式 `Ai / Bi = values[i]` 。每个 `Ai` 或 `Bi` 是一个表示单个变量的字符串。

另有一些以数组 `queries` 表示的问题，其中 `queries[j] = [Cj, Dj]` 表示第 `j` 个问题，请你根据已知条件找出 `Cj / Dj = ?` 的结果作为答案。

返回 **所有问题的答案** 。如果存在某个无法确定的答案，则用 `-1.0` 替代这个答案。如果问题中出现了给定的已知条件中没有出现的字符串，也需要用 `-1.0` 替代这个答案。

**注意：**输入总是有效的。你可以假设除法运算中不会出现除数为 0 的情况，且不存在任何矛盾的结果。

**示例 1：**

```
输入：equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
输出：[6.00000,0.50000,-1.00000,1.00000,-1.00000]
解释：
条件：a / b = 2.0, b / c = 3.0
问题：a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ?
结果：[6.0, 0.5, -1.0, 1.0, -1.0 ]
```

**示例 2：**

```
输入：equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
输出：[3.75000,0.40000,5.00000,0.20000]
```

**示例 3：**

```
输入：equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
输出：[0.50000,2.00000,-1.00000,-1.00000]
```

**提示：**

- `1 <= equations.length <= 20`
- `equations[i].length == 2`
- `1 <= Ai.length, Bi.length <= 5`
- `values.length == equations.length`
- `0.0 < values[i] <= 20.0`
- `1 <= queries.length <= 20`
- `queries[i].length == 2`
- `1 <= Cj.length, Dj.length <= 5`
- `Ai, Bi, Cj, Dj` 由小写英文字母与数字组成

**解法一：并查集**

在 990 题的基础上，加了变量之间的倍数关系

[官方题解](https://leetcode.cn/problems/evaluate-division/solution/399-chu-fa-qiu-zhi-nan-du-zhong-deng-286-w45d/)

```python
class Solution:
    def calcEquation(self, equations: List[List[str]], values: List[float], queries: List[List[str]]) -> List[float]:
        n = len(equations)
        ht = dict()
        idx = 0
        uf = UnionFind(2 * n)
        for (x, y), value in zip(equations, values):
            if x not in ht:
                ht[x] = idx
                idx += 1
            if y not in ht:
                ht[y] = idx
                idx += 1
            uf.union(ht[x], ht[y], value)
        ans = []
        for x, y in queries:
            if x not in ht or y not in ht:
                # print(1)
                ans.append(-1.0)
            else:
                # print(2)
                # print(ht[x], ht[y])
                ans.append(uf.isConnected(ht[x], ht[y]))
        return ans

class UnionFind:
    def __init__(self, n):
        self.ancestor = [i for i in range(n)]
        self.weight = [1.0] * n
    
    def find(self, x):
        if x != self.ancestor[x]:
            x_father = self.ancestor[x]
            self.ancestor[x] = self.find(self.ancestor[x])
            self.weight[x] = self.weight[x] * self.weight[x_father]
        return self.ancestor[x]
    
    # x = value * y
    def union(self, x, y, value):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX == rootY:
            return
        self.ancestor[rootX] = rootY
        self.weight[rootX] = self.weight[y] * value / self.weight[x]
    
    def isConnected(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX == rootY:
            # print(f'{rootX} weight is: {self.weight[rootX]}')
            # print(f'{rootY} weight is: {self.weight[rootY]}')
            # print(f'weight {self.weight}')
            return self.weight[x] / self.weight[y]
        else:
            return -1.0
```

