### [1202. 交换字符串中的元素](https://leetcode.cn/problems/smallest-string-with-swaps/)

- 中等

给你一个字符串 `s`，以及该字符串中的一些「索引对」数组 `pairs`，其中 `pairs[i] = [a, b]` 表示字符串中的两个索引（编号从 0 开始）。

你可以 **任意多次交换** 在 `pairs` 中任意一对索引处的字符。

返回在经过若干次交换后，`s` 可以变成的按字典序最小的字符串。

**示例 1:**

```
输入：s = "dcab", pairs = [[0,3],[1,2]]
输出："bacd"
解释： 
交换 s[0] 和 s[3], s = "bcad"
交换 s[1] 和 s[2], s = "bacd"
```

**示例 2:**

```
输入：s = "dcab", pairs = [[0,3],[1,2],[0,2]]
输出："abcd"
解释：
交换 s[0] 和 s[3], s = "bcad"
交换 s[0] 和 s[2], s = "acbd"
交换 s[1] 和 s[2], s = "abcd"
```

**示例 3:**

```
输入：s = "cba", pairs = [[0,1],[1,2]]
输出："abc"
解释：
交换 s[0] 和 s[1], s = "bca"
交换 s[1] 和 s[2], s = "bac"
交换 s[0] 和 s[1], s = "abc"
```

**解法一：基本并查集**

[并查集讲解](https://zhuanlan.zhihu.com/p/93647900/)

```python
class Solution:
    def smallestStringWithSwaps(self, s: str, pairs: List[List[int]]) -> str:
        n = len(s)
        uf = UnionFind(n)
        for x, y in pairs:
            uf.union(x, y)
        # 建立每个连通分量的代表元 和 字符索引 之间的映射
        # print(uf.ancestor)
        ht = collections.defaultdict(list)
        for i in range(n):
            ht[uf.find(i)].append(s[i])
        # print(ht)
        # 每个子串降序排列，然后每次取的时候 pop 就好了
        for ls in ht.values():
            ls.sort(reverse=True)
        # print(ht)
        ans = []
        # 遍历字符串，每个位置找到对应连通分量中最小的字符
        for i in range(n):
            x = uf.find(i)
            ans.append(ht[x][-1])
            ht[x].pop()
        return ''.join(ans)


class UnionFind:
    def __init__(self, n):
        self.ancestor = [i for i in range(n)]

    def find(self, x):
        if x != self.ancestor[x]:
            self.ancestor[x] = self.find(self.ancestor[x])
        return self.ancestor[x]
    
    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX != rootY:
            self.ancestor[rootX] = self.ancestor[rootY]
```

