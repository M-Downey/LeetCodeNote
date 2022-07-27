### [84. 柱状图中最大的矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/)

- 困难

给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

**示例 1:**

 ![img](https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg)

```
输入：heights = [2,1,5,6,2,3]
输出：10
解释：最大的矩形为图中红色区域，面积为 10
```

**示例 2:**

 ![img](https://assets.leetcode.com/uploads/2021/01/04/histogram-1.jpg)

```
输入： heights = [2,4]
输出： 4
```

**提示：**

- `1 <= heights.length <=105`
- `0 <= heights[i] <= 104`

**解法一：双指针**

对于每一个柱子`heights[i]`，在左边和右边各找到第一个高度比它低的柱子，那么宽就是`right-left-1`高就是`heights[i]`，最大面积就是这俩的乘积，遍历一遍`heights`，取最大面积即可

这个方法时间复杂度太高，不过

**解法二：动态规划**

思路同一，采用数组保存每个柱子左边和右边第一个更低柱子的索引

对于`l_idx`来说，从`1`到`n-1`求，其中第`l_idx[0]`应赋为`-1`

对于`r_idx`来说，从`n-2`到`0`求，其中第`r_idx[n-1]`应赋为`n`

然后从`0`到`n-1`遍历`heights`，因为每个柱子可能单独成矩形，即`i-1`和`i+1`

这时宽是1，高是`heights[i]`

如果左边是`-1`，右边是`n`也是成立的，宽是`n`

```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        n = len(heights)
        l_idx = [-1]*n
        r_idx = [n]*n

        for i in range(1, n):
            # t是左边元素的索引
            t = i-1
            while t>=0 and heights[t] >= heights[i]:
                # 用l_idx更新t更快
                t = l_idx[t]
            l_idx[i] = t

        for i in range(n-2, -1, -1):
            t = i+1
            while t < n and heights[t] >= heights[i]:
                t = r_idx[t]
            r_idx[i] = t

        ans = 0
        for i in range(0, n):
            ans = max(ans, heights[i] *(r_idx[i]-l_idx[i]-1))
        return ans
```

