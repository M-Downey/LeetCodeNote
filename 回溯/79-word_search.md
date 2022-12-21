### [79. 单词搜索](https://leetcode.cn/problems/word-search/)

- 中等

给定一个 `m x n` 二维字符网格 `board` 和一个字符串单词 `word` 。如果 `word` 存在于网格中，返回 `true` ；否则，返回 `false` 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg)

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
输出：true
```

**示例 3：**

 ![img](https://assets.leetcode.com/uploads/2020/10/15/word3.jpg)

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
输出：false
```

**提示：**

- `m == board.length`
- `n = board[i].length`
- `1 <= m, n <= 6`
- `1 <= word.length <= 15`
- `board` 和 `word` 仅由大小写英文字母组成

**解法一：回溯**

**递归参数：**当前检查字母的格子坐标和 `word` 中的索引

**终止条件：**字母不相等，此时返回 `False` （剪枝）；字母都相等，此时 `idx == len(word)` ，此时返回 `True`

**单层逻辑：**将当前格子加入 `visited` ，遍历当前格子四周的格子，如果合法且未访问，就递归检查它，最后要从 `visited` 中移除这个格子

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        m, n = len(board), len(board[0])
        flag = False
        def dfs(sx, sy, idx):
            nonlocal flag
            if word[idx] != board[sx][sy]:
                return
            if idx == len(word) - 1:
                flag = True
                return
            visited.add((sx, sy))
            for x, y in [(sx - 1, sy), (sx + 1, sy), (sx, sy - 1), (sx, sy + 1)]:
                if 0 <= x < m and 0 <= y < n and (x, y) not in visited:
                    dfs(x, y, idx + 1)
            visited.remove((sx, sy))
      
        for i in range(m):
            for j in range(n):
                visited = set()
                dfs(i, j, 0)
                if flag == True:
                    return True
        return False
```

