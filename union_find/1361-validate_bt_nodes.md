### [1361. 验证二叉树](https://leetcode.cn/problems/validate-binary-tree-nodes/)

- 中等

二叉树上有 `n` 个节点，按从 `0` 到 `n - 1` 编号，其中节点 `i` 的两个子节点分别是 `leftChild[i]` 和 `rightChild[i]`。

只有 **所有** 节点能够形成且 **只** 形成 **一颗** 有效的二叉树时，返回 `true`；否则返回 `false`。

如果节点 `i` 没有左子节点，那么 `leftChild[i]` 就等于 `-1`。右子节点也符合该规则。

注意：节点没有值，本问题中仅仅使用节点编号。

**示例 1：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/23/1503_ex1.png)

```
输入：n = 4, leftChild = [1,-1,3,-1], rightChild = [2,-1,-1,-1]
输出：true
```

**示例 2：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/23/1503_ex2.png)

```
输入：n = 4, leftChild = [1,-1,3,-1], rightChild = [2,3,-1,-1]
输出：false
```

**示例 3：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/23/1503_ex3.png)

```
输入：n = 2, leftChild = [1,0], rightChild = [-1,-1]
输出：false
```

**示例 4：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/23/1503_ex4.png)

```
输入：n = 6, leftChild = [1,-1,-1,4,-1,-1], rightChild = [2,-1,-1,5,-1,-1]
输出：false
```

**提示：**

- `1 <= n <= 10^4`
- `leftChild.length == rightChild.length == n`
- `-1 <= leftChild[i], rightChild[i] <= n - 1`

**解法一：并查集**

用并查集维护节点之间的连通性，并查集中要记录连通分量的个数，最后判断连通分量的个数是否是 `1`

这还不够，因为可能是比如三个节点，两个节点指向同一个节点，即：存在入度为 `2` 的节点是不行的。

所以再用一个哈希表记录已经访问过的孩子节点，如果再次访问，就直接返回 `False`

```python
class Solution:
    def validateBinaryTreeNodes(self, n: int, leftChild: List[int], rightChild: List[int]) -> bool:
        uf = UnionFind(n)
        inDegreeSet = set()
        for i in range(n):
            left = leftChild[i]
            if left != -1:
                if left in inDegreeSet:
                    return False
                inDegreeSet.add(left)
                if uf.isConnected(i, left):
                    return False
                else:
                    uf.union(i, left)
            right = rightChild[i]
            if right != -1:
                if right in inDegreeSet:
                    return False
                inDegreeSet.add(right)
                if uf.isConnected(i, right):
                    return False
                else:
                    uf.union(i, right)
        return uf.count == 1

class UnionFind:
    def __init__(self, n):
        self.ancestor = [i for i in range(n)]
        self.size = [1] * n
        self.count = n
    
    def find(self, x):
        if x != self.ancestor[x]:
            self.ancestor[x] = self.find(self.ancestor[x])
        return self.ancestor[x]
    
    # x 是父亲
    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX == rootY:
            return
        if rootX < rootY:
            rootX, rootY = rootY, rootX
        self.ancestor[rootY] = rootX
        self.size[rootX] += self.size[rootY]
        self.count -= 1
    
    def isConnected(self, x, y):
        return self.find(x) == self.find(y)
```

