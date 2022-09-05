### [1582. 二进制矩阵中的特殊位置](https://leetcode.cn/problems/special-positions-in-a-binary-matrix/)

- 简单

给你一个大小为 `rows x cols` 的矩阵 `mat`，其中 `mat[i][j]` 是 `0` 或 `1`，请返回 **矩阵 \*`mat`\* 中特殊位置的数目** 。

**特殊位置** 定义：如果 `mat[i][j] == 1` 并且第 `i` 行和第 `j` 列中的所有其他元素均为 `0`（行和列的下标均**从 0 开始** ），则位置 `(i, j)` 被称为特殊位置。

**示例 1：**

```
输入：mat = [[1,0,0],
            [0,0,1],
            [1,0,0]]
输出：1
解释：(1,2) 是一个特殊位置，因为 mat[1][2] == 1 且所处的行和列上所有其他元素都是 0
```

**示例 2：**

```
输入：mat = [[1,0,0],
            [0,1,0],
            [0,0,1]]
输出：3
解释：(0,0), (1,1) 和 (2,2) 都是特殊位置
```

**示例 3：**

```
输入：mat = [[0,0,0,1],
            [1,0,0,0],
            [0,1,1,0],
            [0,0,0,0]]
输出：2
```

**示例 4：**

```
输入：mat = [[0,0,0,0,0],
            [1,0,0,0,0],
            [0,1,0,0,0],
            [0,0,1,0,0],
            [0,0,0,1,1]]
输出：3
```

**提示：**

- `rows == mat.length`
- `cols == mat[i].length`

- `1 <= rows, cols <= 100`
- `mat[i][j]` 是 `0` 或 `1`

**解法一：模拟**

每个位置如果等一且所在行和列的和也是一，那么就是特殊位置

本题中学到遍历矩阵的列：用 `zip(*mat)`

```python
class Solution:
    def numSpecial(self, mat: List[List[int]]) -> int:
        ans = 0
        m, n = len(mat), len(mat[0])
        row_sum = [sum(row) for row in mat]
        # https://blog.csdn.net/ezio23/article/details/81414092
        # 相当于矩阵转置
        col_sum = [sum(col) for col in zip(*mat)]
        for i in range(m):
            for j in range(n):
                if mat[i][j] == 1 and row_sum[i] == 1 and col_sum[j] == 1:
                    ans += 1
        return ans
```

