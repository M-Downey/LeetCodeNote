### [73. 矩阵置零](https://leetcode.cn/problems/set-matrix-zeroes/)

给定一个 `m x n` 的矩阵，如果一个元素为 **0** ，则将其所在行和列的所有元素都设为 **0** 。请使用 **[原地](http://baike.baidu.com/item/原地算法)** 算法。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg)

```
输入：matrix = [[1,1,1],[1,0,1],[1,1,1]]
输出：[[1,0,1],[0,0,0],[1,0,1]]
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg)

![img](https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg)

```
输入：matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
输出：[[0,0,0,0],[0,4,5,0],[0,3,1,0]]
```

提示：

- `m == matrix.length`
- `n == matrix[0].length`
- `1 <= m, n <= 200`
- `-231 <= matrix[i][j] <= 231 - 1`

进阶：

- 一个直观的解决方案是使用  O(mn) 的额外空间，但这并不是一个好的解决方案。
- 一个简单的改进方案是使用 O(m + n) 的额外空间，但这仍然不是最好的解决方案。
- 你能想出一个仅使用常量空间的解决方案吗？

**解法一：**先遍历一遍矩阵，标记为0的元素的行列，然后再遍历一次，如果处在0元素的行列，则该元素置0

时间复杂度：`O(m * n)`， (循环)

空间复杂度：`O(m + n)`， (行，列标记数组的大小)

```python
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        m, n = len(matrix), len(matrix[0])
        rows = [0] * m
        cols = [0] * n
        for i in range(m):
            for j in range(n):
                if matrix[i][j] == 0:
                    rows[i] = 1
                    cols[j] = 1
        for i in range(m):
            for j in range(n):
                if rows[i] == 1 or cols[j] == 1:
                    matrix[i][j] = 0
        return matrix
```

