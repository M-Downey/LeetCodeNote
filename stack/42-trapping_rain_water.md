### [42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)

- 困难

给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

**示例 1：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)

```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
```

**示例 2：**

```
输入：height = [4,2,0,3,2,5]
输出：9
```

**提示：**

- `n == height.length`
- `1 <= n <= 2 * 10^4`
- `0 <= height[i] <= 10^5`

**解法一：双指针**

**如果按照列来计算的话，宽度一定是1了，我们再把每一列的雨水的高度求出来就可以了。**

可以看出每一列雨水的高度，取决于，该列 左侧最高的柱子和右侧最高的柱子中最矮的那个柱子的高度。

 ![42.接雨水3](https://img-blog.csdnimg.cn/20210223092732301.png)

那么列4的雨水高度为 列3和列7的高度最小值减列4高度，即： `min(lHeight, rHeight) - height`。

并且**要注意第一个柱子和最后一个柱子不接雨水**

会超时

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        n = len(height)
        sum = 0
        for i in range(1, n-1):
            lh, rh = height[i], height[i]
            for l in range(0, i):
                lh = height[l] if height[l] > lh else lh
            for r in range(i+1, n):
                rh = height[r] if height[r] > rh else rh
            hi = min(lh, rh) - height[i]
            if hi > 0:
                sum += hi
        return sum
```

**解法二：动态规划**

用数组保存每个位置的左边，右边的最大值

左边最大值，从`1`到`n-1`，`maxl[0]=height[0]`

右边最大值，从`0`到`n-2`，`maxr[n-1]=height[n-1]`

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        n = len(height)
        lmax, rmax = [0]*n, [0]*n
        lmax[0] = height[0]
        rmax[n-1] = height[n-1]
        for i in range(1, n):
            lmax[i] = max(lmax[i-1], height[i])
            rmax[n-1-i] = max(rmax[n-1-i+1], height[n-1-i])
        
        sum = 0
        for i in range(1, n):
            if min(lmax[i], rmax[i]) > height[i]:
                sum += min(lmax[i], rmax[i]) - height[i]
        return sum
```

**解法三：单调栈**

