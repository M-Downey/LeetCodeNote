### [200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/)

- 中等

给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

**示例 1：**

```
输入：grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
输出：1
```

**示例 2：**

```
输入：grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
输出：3
```

**提示：**

- `m == grid.length`
- `n == grid[i].length`

- `1 <= m, n <= 300`
- `grid[i][j]` 的值为 `'0'` 或 `'1'`

**解法一：DFS**

DFS遍历每一个岛，将遍历到的位置都置为 `0` ，这样遍历的次数就是岛屿的个数

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        m, n = len(grid), len(grid[0])
        ans = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1':
                    ans += 1
                    self.dfs(grid, i, j)
        return ans

    def dfs(self, grid, row, col):
        grid[row][col] = 0
        for r, c in [(row - 1, col), (row + 1, col), (row, col - 1), (row, col + 1)]:
            if 0 <= r < len(grid) and 0 <= c < len(grid[0]) and grid[r][c] == '1':
                self.dfs(grid, r, c)
```

Java 版本的语法一言难尽。遍历每个方向时，要用二维数组。visited 集合不能用 map 作为元素，问题很多（比如 map 和元组不一样 (1, 2) 和(1,3) 应该都能保存但是map不能）可以用 x+","+y 的字符串作为每个节点的集合元素。

优化版本：访问过的节点直接置‘0’，就不用 visited 集合了

```java
class Solution {
    Set<String> visited = new HashSet<>();

    public int numIslands(char[][] grid) {
        int ans = 0;
        int m = grid.length;
        int n = grid[0].length;
        // System.out.println(m + " " + n);
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1' && !visited.contains(i + "," + j)) {
                    System.out.println(i+","+j);
                    ans++;
                    dfs(i, j, grid);
                }
            }
        }
        return ans;
    }

    // 遍历 (x, y) 为源点的陆地
    public void dfs(int x, int y, char[][] grid) {
        visited.add(x + "," + y);
        int m = grid.length;
        int n = grid[0].length;
        // 遍历 (x, y) 所有相邻节点
        Map<Integer, Integer> dirs = new HashMap<>();
        dirs.put(-1, 0);
        dirs.put(0, -1);
        dirs.put(0, 1);
        dirs.put(1, 0);
        for (Map.Entry<Integer, Integer> entry : dirs.entrySet()) {
            int nx = x + entry.getKey();
            int ny = y + entry.getValue();
            if (nx < 0 || nx >= m || ny < 0 || ny >= n) {
                continue;
            }
            if (grid[nx][ny] == '1' && !visited.contains(nx + "," + ny)) {
                // System.out.println(nx + "," + ny);
                dfs(nx, ny, grid);
            }
        }
    }
}

// 优化掉 visited 数组版本
class Solution {
    public int numIslands(char[][] grid) {
        int ans = 0;
        int m = grid.length;
        int n = grid[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    dfs(i, j, grid);
                    ans++;
                }
            }
        }
        return ans;
    }

    public void dfs(int x, int y, char[][] grid) {
        grid[x][y] = '0';
        int[][] dirs = {{-1, 0}, {0, -1}, {0, 1}, {1, 0}};
        int m = grid.length;
        int n = grid[0].length;
        for (int[] dir : dirs) {
            int nx = x + dir[0];
            int ny = y + dir[1];
            if (nx >= 0 && nx < m && ny >= 0 && ny < n && grid[nx][ny] == '1') {
                dfs(nx, ny, grid);
            }
        }
    }
}
```



**解法二：BFS**

**解法二：BFS**

为了求出岛屿的数量，我们可以扫描整个二维网格。如果一个位置为 1，则将其加入队列，开始进行广度优先搜索。在广度优先搜索的过程中，**每个搜索到的 1 都会被重新标记为 0**。直到队列为空，搜索结束。

最终岛屿的数量就是我们进行广度优先搜索的次数。

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        m, n = len(grid), len(grid[0])
        cnt = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == '1':
                    grid[i][j] = '0'
                    q = deque()
                    q.append((i, j))
                    while q:
                        row, col = q.popleft()
                        for r, c in [(row - 1, col), (row + 1, col), (row, col - 1), (row, col + 1)]:
                            if 0 <= r < m and 0 <= c < n and grid[r][c] == '1':
                                q.append((r, c))
                                # 走过的路径置0!!!
                                grid[r][c] = '0'
                    cnt += 1
        return cnt
```

**解法三：并查集**

为了求出岛屿的数量，我们可以扫描整个二维网格。如果一个位置为 11，则将其与相邻四个方向上的 11 在并查集中进行合并。

最终岛屿的数量就是并查集中连通分量的数目。

```python
class UnionFind:
    def __init__(self, grid):
        m, n = len(grid), len(grid[0])
        self.count = 0
        self.parent = [-1] * (m * n)
        for i in range(m):
            for j in range(n):
                if grid[i][j] == "1":
                    self.parent[i * n + j] = i * n + j
                    self.count += 1
    
    def find(self, i):
        if self.parent[i] != i:
            self.parent[i] = self.find(self.parent[i])
        return self.parent[i]
    
    def union(self, x, y):
        rootx = self.find(x)
        rooty = self.find(y)
        if rootx != rooty:
            self.parent[rooty] = rootx
            self.count -= 1
    
    def getCount(self):
        return self.count

class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        nr = len(grid)
        if nr == 0:
            return 0
        nc = len(grid[0])

        uf = UnionFind(grid)

        for r in range(nr):
            for c in range(nc):
                if grid[r][c] == "1":
                    # grid[r][c] = "0"
                    for x, y in [(r - 1, c), (r + 1, c), (r, c - 1), (r, c + 1)]:
                        if 0 <= x < nr and 0 <= y < nc and grid[x][y] == "1":
                            uf.union(r * nc + c, x * nc + y)
        
        return uf.getCount()
```

```java
class Solution {
    class UnionFind {

        int[] parent;
        int cnt;

        public UnionFind(int n, int count) {
            parent = new int[n];
            cnt = count;
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
        }
        
        public int find(int x) {
            if (parent[x] != x) {
                parent[x] = find(parent[x]);
            }
            return parent[x];
        }
        
        public void union(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);
            if (rootX == rootY) {
                return;
            }
            parent[rootY] = rootX;
            cnt--;
        }
    }

    public int numIslands(char[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int count = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    count++;
                }
            }
        }
        UnionFind uf = new UnionFind(m * n, count);

        int[][] dirs = {{-1, 0}, {0, -1}, {0, 1}, {1, 0}};
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 每个陆地点的周围陆地点，都连接在一起
                if (grid[i][j] == '1') {
                    for (int[] dir : dirs) {
                        int x = dir[0] + i;
                        int y = dir[1] + j;
                        if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == '1') {
                            uf.union(i * n + j, x * n + y);
                        }
                    }
                }
            }
        }
        return uf.cnt;
    }
}
```

