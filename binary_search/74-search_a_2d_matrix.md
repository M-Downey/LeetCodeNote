### [74. 搜索二维矩阵](https://leetcode.cn/problems/search-a-2d-matrix/)

- 中等

编写一个高效的算法来判断 `m x n` 矩阵中，是否存在一个目标值。该矩阵具有如下特性：

- 每行中的整数从左到右按升序排列。
- 每行的第一个整数大于前一行的最后一个整数。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2020/10/05/mat.jpg)

```
输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
输出：true
```

**示例 2：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/25/mat2.jpg)

```
输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
输出：false
```

**提示：**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 100`
- `-10^4 <= matrix[i][j], target <= 10^4`

**解法一：二分**

要明确：在第一列中，是要找 `target` 的右边界，那么在二分时，只要当前元素小于等于 `target` 就要往右走

[`bisect` 源码](https://blog.csdn.net/weixin_43955170/article/details/119085829)

- 手动实现：

```
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m, n = len(matrix), len(matrix[0])
        up, down = 0, m - 1
        # 要找到 target 的右边界
        while up <= down:
            mid = (up + down) // 2
            if matrix[mid][0] <= target:
                up = mid + 1
            else:
                down = mid - 1
        # print(up)
        left, right = 0, n - 1
        while left <= right:
            mid = (left + right) // 2
            if matrix[up - 1][mid] == target:
                return True
            elif matrix[up - 1][mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        return False
```

- 库函数实现：

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m, n = len(matrix), len(matrix[0])
        column = [matrix[i][0] for i in range(m)]
        idx = bisect.bisect_right(column, target)
        left, right = 0, n - 1
        while left <= right:
            mid = (left + right) // 2
            if matrix[idx - 1][mid] == target:
                return True
            elif matrix[idx - 1][mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        return False
```

