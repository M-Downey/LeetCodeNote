### [864. 获取所有钥匙的最短路径](https://leetcode.cn/problems/shortest-path-to-get-all-keys/)

- 困难

给定一个二维网格 `grid` ，其中：

- '.' 代表一个空房间
- '#' 代表一堵
- '@' 是起点
- 小写字母代表钥匙
- 大写字母代表锁

我们从起点开始出发，一次移动是指向四个基本方向之一行走一个单位空间。我们不能在网格外面行走，也无法穿过一堵墙。如果途经一个钥匙，我们就把它捡起来。除非我们手里有对应的钥匙，否则无法通过锁。

假设 k 为 钥匙/锁 的个数，且满足 `1 <= k <= 6`，字母表中的前 `k` 个字母在网格中都有自己对应的一个小写和一个大写字母。换言之，每个锁有唯一对应的钥匙，每个钥匙也有唯一对应的锁。另外，代表钥匙和锁的字母互为大小写并按字母顺序排列。

返回获取所有钥匙所需要的移动的最少次数。如果无法获取所有钥匙，返回 `-1` 。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2021/07/23/lc-keys2.jpg)

```
输入：grid = ["@.a.#","###.#","b.A.B"]
输出：8
解释：目标是获得所有钥匙，而不是打开所有锁。
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2021/07/23/lc-key2.jpg)

```
输入：grid = ["@..aA","..B#.","....b"]
输出：6
```

**示例 3：**

 ![img](https://assets.leetcode.com/uploads/2021/07/23/lc-keys3.jpg)

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 30`
- `grid[i][j]` 只含有 `'.'`, `'#'`, `'@'`, `'a'-``'f``'` 以及 `'A'-'F'`
- 钥匙的数目范围是 `[1, 6]` 
- 每个钥匙都对应一个 **不同** 的字母
- 每个钥匙正好打开一个对应的锁

**解法一：BFS+状态压缩**

给定一个只包含空房间、墙、起点和终点的二维网格，我们可以使用广度优先搜索的方法求出起点到终点的最短路径。这是因为在最短路径上，我们最多只会经过每个房间一次。因此从起点开始，使用队列进行广度优先搜索，当第一个搜索到某个节点的时候，我们就可以得到从起点到该节点正确的最短路。

如果加上了钥匙和锁，我们应该如何解决问题呢？类似地，在最短路径上也不可能存在如下的情况：我们经过了某个房间两次，并且这两次我们拥有钥匙的情况是完全一致的。

因此，我们可以用一个三元组 `(x, y, mask)` 表示当前的状态，其中 `(x, y)` 表示当前所处的位置，$\textit{mask}$ 是一个二进制数，长度恰好等于网格中钥匙的数目，$\textit{mask}$ 的第 i 个二进制位为 1，当且仅当我们已经获得了网格中的第 i 把钥匙。

这样一来，我们就可以使用上述的状态进行广度优先搜索。初始时，我们把 `(sx, sy, 0)` 加入队列，其中 `(sx, sy)` 为起点。在搜索的过程中，我们可以向上下左右四个方向进行扩展：

如果对应方向是空房间，那么 mask 的值不变；

如果对应方向是第 i 把钥匙，那么将 mask 的第 i 位置为 1；

如果对应方向是第 i 把锁，那么只有在 mask 的第 i 位为 1 时，才可以通过。

当我们搜索到一个 $\textit{mask}$ 每一个二进制都为 1 的状态时，说明获取了所有钥匙，此时就可以返回最短路作为答案。

```python
class Solution:
    def shortestPathAllKeys(self, grid: List[str]) -> int:
        # BFS + 状态压缩
        # 遇到普通房间，未访问，加入队列
        # 遇到钥匙，未访问，记录钥匙信息，加入队列；检查钥匙个数，集齐了就返回路径长度
        # 遇到房间，未访问，检查钥匙信息，加入队列
        m, n = len(grid), len(grid[0])
        sx, sy = 0, 0
        key_to_idx = dict()
        # 记录起点和钥匙
        for i in range(m):
            for j in range(n):
                if grid[i][j] == '@':
                    sx, sy = i, j
                elif grid[i][j].islower():
                    key_to_idx[grid[i][j]] = len(key_to_idx)
        q = deque([(sx, sy, 0)])
        dist = dict()
        dist[(sx, sy, 0)] = 0
        dirs = [(-1, 0), (0, 1), (1, 0), (0, -1)]
        while q:
            x, y, mask = q.popleft()
            for dx, dy in dirs:
                nx, ny = x + dx, y + dy
                # 下一步合法且不是墙
                if 0 <= nx < m and 0 <= ny < n and grid[nx][ny] != '#':
                    # 下一格是空房间或起点且没访问过
                    # !!! 注意：起点不能直接舍去，因为可以原路返回（但钥匙状态不同了）
                    if (grid[nx][ny] == '.' or grid[nx][ny] =='@') and (nx, ny, mask) not in dist:
                        dist[(nx, ny, mask)] = dist[(x, y, mask)] + 1
                        q.append((nx, ny, mask))
                    # 如果是钥匙
                    elif grid[nx][ny].islower():
                        idx = key_to_idx[grid[nx][ny]]
                        if (nx, ny, mask | (1 << idx)) not in dist:
                            dist[(nx, ny, mask | (1 << idx))] = dist[(x, y, mask)] + 1
                            # print((mask | (1 << idx)), (1 << len(key_to_idx)) - 1)
                            if (mask | (1 << idx)) == ((1 << len(key_to_idx)) - 1):
                                # print('right')
                                return dist[(nx, ny, mask | (1 << idx))]
                            q.append((nx, ny, mask | (1 << idx)))
                    # 如果是锁，检查对应的钥匙
                    elif grid[nx][ny].isupper():
                        idx = key_to_idx[grid[nx][ny].lower()]
                        if (mask & (1 << idx)) and (nx, ny, mask) not in dist:
                            dist[(nx, ny, mask)] = dist[(x, y, mask)] + 1
                            q.append((nx, ny, mask))
            # print(q)
        return -1
```

