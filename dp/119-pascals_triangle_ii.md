### [119. 杨辉三角 II](https://leetcode.cn/problems/pascals-triangle-ii/)

- 简单

给定一个非负索引 `rowIndex`，返回「杨辉三角」的第 `rowIndex` 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

 ![img](https://pic.leetcode-cn.com/1626927345-DZmfxB-PascalTriangleAnimated2.gif)

**示例 1:**

```
输入: rowIndex = 3
输出: [1,3,3,1]
```

**示例 2:**

```
输入: rowIndex = 0
输出: [1]
```

**示例 3:**

```
输入: rowIndex = 1
输出: [1,1]
```

**解法一：**同杨辉三角一，求出所有结果后返回第n行，略去

**解法二：**空间复杂度`O(rowIndex)`的方法

在`row[rowIndex+1]`的赋初始值1的基础上，通过迭代为更新其值

共有`rowIndex+1`行，需迭代`rowIndex-1`次，因为第0，1行不用更新

故循环`rowIndex-1`次

**每次更新一行，需要从后往前更新**

因为递推公式为：`row[j] = row[j] + row[j-1]`需要用到当前值的前一个值

**重点：**

第`rowIndex`行，有`rowIndex-1`个需要更新的，从索引`rowIndex-1`开始，到`1`结束

故`j`从`1`到`i-1`，索引从`i-1`到`1`

```python
class Solution:
    def getRow(self, rowIndex: int) -> List[int]:
        row = [1] * (rowIndex+1)
        for i in range(2, rowIndex+1):
            for j in range(1, i):
                row[i-j] = row[i-j] + row[i-j-1]
        return row
```

