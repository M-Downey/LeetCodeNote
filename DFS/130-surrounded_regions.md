### [130. 被围绕的区域](https://leetcode.cn/problems/surrounded-regions/)

- 中等

给你一个 `m x n` 的矩阵 `board` ，由若干字符 `'X'` 和 `'O'` ，找到所有被 `'X'` 围绕的区域，并将这些区域里所有的 `'O'` 用 `'X'` 填充。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2021/02/19/xogrid.jpg)

```
输入：board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]
输出：[["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]
解释：被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。
```

**示例 2：**

```
输入：board = [["X"]]
输出：[["X"]]
```

**提示：**

- `m == board.length`
- `n == board[i].length`

- `1 <= m, n <= 200`
- `board[i][j]` 为 `'X'` 或 `'O'`

**解法一：DFS**

把边界的 `'O'` 都 `DFS` 遍历成 `'A'` ，然后把剩余的 `'O'` 遍历成 `'X'` ，最后再把边界 `'A'` 改回 `'O'`

```python
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        m, n = len(board), len(board[0])
        for i in range(m):
            for j in range(n):
                if i == 0 or i == m - 1 or j == 0 or j == n - 1:
                    self.dfs(board, i, j)
        for i in range(m):
            for j in range(n):
                if board[i][j] == 'O':
                    board[i][j] = 'X'
        for i in range(m):
            for j in range(n):
                if board[i][j] == 'A':
                    board[i][j] = 'O'

    # 把边界的 'O' 都转换成 'A'
    def dfs(self, board, row, col):
        m, n = len(board), len(board[0])
        if 0 <= row < m and 0 <= col < n and board[row][col] == 'O':
            board[row][col] = 'A'
            self.dfs(board, row - 1, col)
            self.dfs(board, row + 1, col)
            self.dfs(board, row, col - 1)
            self.dfs(board, row, col + 1)
```

