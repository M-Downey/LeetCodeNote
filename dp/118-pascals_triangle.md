### [118. 杨辉三角](https://leetcode.cn/problems/pascals-triangle/)

- 简单

给定一个非负整数 *`numRows`，*生成「杨辉三角」的前 *`numRows`* 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

 ![img](https://pic.leetcode-cn.com/1626927345-DZmfxB-PascalTriangleAnimated2.gif)

**示例 1:**

```
输入: numRows = 5
输出: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```

**示例 2:**

```
输入: numRows = 1
输出: [[1]]
```

**提示:**

- `1 <= numRows <= 30`

**解法一：**

```python
class Solution:
    def generate(self, numRows: int) -> List[List[int]]:
        res = [[1] for _ in range(numRows)]
        # 求第i行的
        for i in range(1, numRows):
            # 求i行，要看i-1行的
            for j in range(0, i - 1):
                res[i].append(res[i - 1][j] + res[i - 1][j + 1])
            res[i].append(1)
        return res
```

