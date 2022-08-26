### [684. 冗余连接](https://leetcode.cn/problems/redundant-connection/)

- 中等

树可以看成是一个连通且 **无环** 的 **无向** 图。

给定往一棵 `n` 个节点 (节点值 `1～n`) 的树中添加一条边后的图。添加的边的两个顶点包含在 `1` 到 `n` 中间，且这条附加的边不属于树中已存在的边。图的信息记录于长度为 `n` 的二维数组 `edges` ，`edges[i] = [ai, bi]` 表示图中在 `ai` 和 `bi` 之间存在一条边。

请找出一条可以删去的边，删除后可使得剩余部分是一个有着 `n` 个节点的树。如果有多个答案，则返回数组 `edges` 中最后出现的边。

**示例 1：**

 ![img](https://pic.leetcode-cn.com/1626676174-hOEVUL-image.png)

```
输入: edges = [[1,2], [1,3], [2,3]]
输出: [2,3]
```

**示例 2：**

 ![img](https://pic.leetcode-cn.com/1626676179-kGxcmu-image.png)

```
输入: edges = [[1,2], [2,3], [3,4], [1,4], [1,5]]
输出: [1,4]
```

**提示:**

- `n == edges.length`
- `3 <= n <= 1000`

- `edges[i].length == 2`
- `1 <= ai < bi <= edges.length`

- `ai != bi`
- `edges` 中无重复元素
- 给定的图是连通的 

**解法一：并查集**

遍历每一条边的节点，如果两节点属于不同集合，就把两集合合并，否则，**说明该边连接了两个集合**，该边就是多余的边

```python
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        n = len(edges)
        parent = [i for i in range(n + 1)]
        def find(x):
            if x == parent[x]:
                return x
            else:
                return find(parent[x])
        def union(u, v):
            u = find(u)
            v = find(v)
            parent[u] = v
        for node1, node2 in edges:
            if find(node1) != find(node2):
                union(node1, node2)
            else:
                return [node1, node2]
```

路径压缩后

路径压缩是指：在查询某个元素的根节点时，同时把该元素的根节点赋值为父节点的根节点（可以降低树的高度）

```python
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        n = len(edges)
        parent = [i for i in range(n + 1)]
        def find(x):
            if x != parent[x]:
                parent[x] = find(parent[x])
            return parent[x]
        def union(u, v):
            u = find(u)
            v = find(v)
            parent[u] = v
        for node1, node2 in edges:
            if find(node1) != find(node2):
                union(node1, node2)
            else:
                return [node1, node2]
```

