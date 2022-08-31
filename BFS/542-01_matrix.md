### [542. 01 矩阵](https://leetcode.cn/problems/01-matrix/)

- 中等

给定一个由 `0` 和 `1` 组成的矩阵 `mat` ，请输出一个大小相同的矩阵，其中每一个格子是 `mat` 中对应位置元素到最近的 `0` 的距离。

两个相邻元素间的距离为 `1` 。

**示例 1：**

 ![img](https://pic.leetcode-cn.com/1626667201-NCWmuP-image.png)

```
输入：mat = [[0,0,0],[0,1,0],[0,0,0]]
输出：[[0,0,0],[0,1,0],[0,0,0]]
```

**示例 2：**

 ![img](https://pic.leetcode-cn.com/1626667205-xFxIeK-image.png)

```
输入：mat = [[0,0,0],[0,1,0],[1,1,1]]
输出：[[0,0,0],[0,1,0],[1,2,1]]
```

**提示：**

- `m == mat.length`
- `n == mat[i].length`

- `1 <= m, n <= 10^4`
- `1 <= m * n <= 10^4`

- `mat[i][j] is either 0 or 1.`
- `mat` 中至少有一个 `0 `

**解法一：BFS**

每个 `1` 周围最近的 `0` 的距离和每个 `0` 周围到 `1` 的距离是相同的

从所有的 `0` 出发开始 `BFS` ，赋值距离即可

```python
class Solution:
    def updateMatrix(self, mat: List[List[int]]) -> List[List[int]]:
        m, n = len(mat), len(mat[0])
        ans = [[0 for _ in range(n)] for _ in range(m)]
        zeros = [(i, j) for i in range(m) for j in range(n) if mat[i][j] == 0]
        visited = set(zeros)
        q = deque(zeros)
        while q:
            row, col = q.popleft()
            for r, c in [(row - 1, col), (row + 1, col), (row, col - 1), (row, col + 1)]:
                if 0 <= r < m and 0 <= c < n and (r, c) not in visited:
                    q.append((r, c))
                    visited.add((r, c))
                    ans[r][c] = ans[row][col] + 1
        return ans
```

