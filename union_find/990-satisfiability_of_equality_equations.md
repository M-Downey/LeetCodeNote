### [990. 等式方程的可满足性](https://leetcode.cn/problems/satisfiability-of-equality-equations/)

- 中等

给定一个由表示变量之间关系的字符串方程组成的数组，每个字符串方程 `equations[i]` 的长度为 `4`，并采用两种不同的形式之一：`"a==b"` 或 `"a!=b"`。在这里，a 和 b 是小写字母（不一定不同），表示单字母变量名。

只有当可以将整数分配给变量名，以便满足所有给定的方程时才返回 `true`，否则返回 `false`。 

**示例 1：**

```
输入：["a==b","b!=a"]
输出：false
解释：如果我们指定，a = 1 且 b = 1，那么可以满足第一个方程，但无法满足第二个方程。没有办法分配变量同时满足这两个方程。
```

**示例 2：**

```
输入：["b==a","a==b"]
输出：true
解释：我们可以指定 a = 1 且 b = 1 以满足满足这两个方程。
```

**示例 3：**

```
输入：["a==b","b==c","a==c"]
输出：true
```

**示例 4：**

```
输入：["a==b","b!=c","c==a"]
输出：false
```

**示例 5：**

```
输入：["c==c","b==d","x!=z"]
输出：true
```

**提示：**

1. `1 <= equations.length <= 500`
2. `equations[i].length == 4`
3. `equations[i][0]` 和 `equations[i][3]` 是小写字母
4. `equations[i][1]` 要么是 `'='`，要么是 `'!'`
5. `equations[i][2]` 是 `'='`

**解法一：并查集**

字母要映射成索引，等号的字母连接在一起，然后再检查不等号的字母，如果是连接的，就返回 `False`

```python
class Solution:
    def equationsPossible(self, equations: List[str]) -> bool:
        ht = dict()
        idx = 0
        uf = UnionFind(26)
        for x, sign, _, y in equations:
            if x not in ht:
                ht[x] = idx
                idx += 1
            if y not in ht:
                ht[y] = idx
                idx += 1
            if sign == '=':
                uf.union(ht[x], ht[y])
        for x, sign, _, y in equations:
            if sign == '!':
                if uf.isConnected(ht[x], ht[y]):
                    return False
        return True

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
        if rootX < rootY:
            rootX, rootY = rootY, rootX
        self.ancestor[rootY] = rootX
        self.size[rootX] += self.size[rootY]
    
    def isConnected(self, x, y):
        return self.find(x) == self.find(y)
```

