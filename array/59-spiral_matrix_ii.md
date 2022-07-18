## [59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/)

- 中等

这题图一乐

给你一个正整数 `n` ，生成一个包含 `1` 到 `n2` 所有元素，且元素按顺时针顺序螺旋排列的 `n x n` 正方形矩阵 `matrix` 。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2020/11/13/spiraln.jpg)

```
输入：n = 3
输出：[[1,2,3],[8,9,4],[7,6,5]]
```

**示例 2：**

```
输入：n = 1
输出：[[1]]
```

**提示：**

- `1 <= n <= 20`

**解法一：模拟**

```python
# 本题收获：
# 对于for i in range(a, b, -1)
# 从i从a遍历到b+1
```

```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        l, r, t, b = 0, n-1, 0, n-1
        matrix = [[0]*n for _ in range(n)]
        num = 1
        while num <= n*n:
            for i in range(l, r+1):
                matrix[t][i] = num
                num += 1
            t += 1
            for i in range(t, b+1):
                matrix[i][r] = num
                num += 1
            r -= 1
            for i in range(r, l-1, -1):
                matrix[b][i] = num
                num += 1
            b -= 1
            for i in range(b, t-1, -1):
                matrix[i][l] = num
                num += 1
            l += 1
        return matrix
```

