### [675. 为高尔夫比赛砍树](https://leetcode.cn/problems/cut-off-trees-for-golf-event/)

- 困难

你被请来给一个要举办高尔夫比赛的树林砍树。树林由一个 `m x n` 的矩阵表示， 在这个矩阵中：

- `0` 表示障碍，无法触碰
- `1` 表示地面，可以行走
- `比 1 大的数` 表示有树的单元格，可以行走，数值表示树的高度（！！！）

每一步，你都可以向上、下、左、右四个方向之一移动一个单位，如果你站的地方有一棵树，那么你可以决定是否要砍倒它。

你需要按照树的高度从低向高砍掉所有的树，每砍过一颗树，该单元格的值变为 `1`（即变为地面）。

你将从 `(0, 0)` 点开始工作，返回你砍完所有树需要走的最小步数。 如果你无法砍完所有的树，返回 `-1` 。

可以保证的是，没有两棵树的高度是相同的，并且你至少需要砍倒一棵树。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2020/11/26/trees1.jpg)

```
输入：forest = [[1,2,3],[0,0,4],[7,6,5]]
输出：6
解释：沿着上面的路径，你可以用 6 步，按从最矮到最高的顺序砍掉这些树。
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2020/11/26/trees2.jpg)

```
输入：forest = [[1,2,3],[0,0,0],[7,6,5]]
输出：-1
解释：由于中间一行被障碍阻塞，无法访问最下面一行中的树。
```

**示例 3：**

```
输入：forest = [[2,3,4],[0,0,5],[8,7,6]]
输出：6
解释：可以按与示例 1 相同的路径来砍掉所有的树。
(0,0) 位置的树，可以直接砍去，不用算步数。
```

**提示：**

- `m == forest.length`
- `n == forest[i].length`

- `1 <= m, n <= 50`
- `0 <= forest[i][j] <= 10^9`

**解法一：BFS**

因为路径要砍了所有树，并且从低到高砍，所以如果路径存在，**一定唯一**

从 `(0, 0)` 出发到走到下一颗树，**是可以走树的**

所以问题转换为，求高度相邻的树（从低到高）之间的最短距离，用 `BFS` 求

最后加和，如果有走不到的情况，直接返回 `-1` 

```python
class Solution:
    def cutOffTree(self, forest: List[List[int]]) -> int:
        # 树是可以走的，如果存在答案，结果一定唯一
        # 记录所有树
        trees = sorted([(h, i, j) for i, row in enumerate(forest) for j, h in enumerate(row) if h > 1])
        m, n = len(forest), len(forest[0])
        # 从 (sx, sy) 到 (x, y) 的最短距离
        def bfs(sx, sy, x, y):
            q = deque([(0, sx, sy)])
            visited = set((sx, sy))
            res = -1
            while q:
                dis, row, col = q.popleft()
                if row == x and col == y:
                    return dis
                for r, c in [(row - 1, col), (row + 1, col), (row, col - 1), (row, col + 1)]:
                    if 0 <= r < m and 0 <= c < n and forest[r][c] and (r, c) not in visited:
                        q.append((dis + 1, r, c))
                        visited.add((r, c))
            return -1
        
        ans, preI, preJ = 0, 0, 0
        for h, i, j in trees:
            d = bfs(preI, preJ, i, j)
            if d < 0:
                return -1
            ans += d
            preI, preJ = i, j
        return ans
```

