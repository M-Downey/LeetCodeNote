### [1739. 放置盒子](https://leetcode.cn/problems/building-boxes/)

- 困难

有一个立方体房间，其长度、宽度和高度都等于 `n` 个单位。请你在房间里放置 `n` 个盒子，每个盒子都是一个单位边长的立方体。放置规则如下：

- 你可以把盒子放在地板上的任何地方。
- 如果盒子 `x` 需要放置在盒子 `y` 的顶部，那么盒子 `y` 竖直的四个侧面都 **必须** 与另一个盒子或墙相邻。

给你一个整数 `n` ，返回接触地面的盒子的 **最少** 可能数量。

**示例 1：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/24/3-boxes.png)

```
输入：n = 3
输出：3
解释：上图是 3 个盒子的摆放位置。
这些盒子放在房间的一角，对应左侧位置。
```

**示例 2：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/24/4-boxes.png)

```
输入：n = 4
输出：3
解释：上图是 3 个盒子的摆放位置。
这些盒子放在房间的一角，对应左侧位置。
```

**示例 3：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/24/10-boxes.png)

```
输入：n = 10
输出：6
解释：上图是 10 个盒子的摆放位置。
这些盒子放在房间的一角，对应后方位置。
```

**提示：**

- `1 <= n <= 10^9`

**解法一：数学+找规律**

[官方分析](https://leetcode.cn/problems/building-boxes/solution/fang-zhi-he-zi-by-leetcode-solution-7ah2/)

```python
class Solution:
    def minimumBoxes(self, n: int) -> int:
        cur = i = j = 1
        while n > cur:
            n -= cur
            i += 1
            cur += i
        cur = 1
        while n > cur:
            n -= cur
            j += 1
            cur += 1
        return (i - 1) * i // 2 + j
```

**解法二：找规律+二分**

```python
class Solution:
    def minimumBoxes(self, n: int) -> int:
        # 二分查找
        # 设 f(x) = x*(x+1)/2 是当在底部添加 x 个盒子能总共多添加的盒子数
        # 设 g(i) = sum(f(1), f(2), f(i)) = i*(i+1)*(i+2)/6 (从1到n的平方和公式推导)
        # 是前 i 层盒子的数量
        # 则先找到一个i - 1层，使得前 i-1 层填满盒子，第 i 层可能没填满的情况
        # 即 g(i) >= n
        # 再从剩余的 n - g(i-1) 中找到底部新增盒子 j 的大小
        # 第 i 层底部新增盒子 j 范围是 [1, i]
        # 要找到满足 f(j) >= n - g(i - 1) 的最小 j
        # 最后计算底部盒子数 i-1 层 +j 个
        # 因为 n <= 10**9 答案最多达到 sqrt(n) 的数量级，可以取右边界上限是 10**5
        left, right = 1, min(n, 10**5)
        while left < right:
            mid = (left + right) // 2
            s = mid * (mid + 1) * (mid + 2) // 6
            if s >= n:
                right = mid
            else:
                left = mid + 1
        i = right
        # 减去前 i-1 层的 g(i - 1)
        n -= (i - 1) * i * (i + 1) // 6
        # print(i, n)

        left, right = 1, i
        while left < right:
            mid = (left + right) // 2
            s = mid * (mid + 1) // 2
            if s >= n:
                right = mid
            else:
                left = mid + 1
        j = right
        # 前 i-1 层 +j
        ans = i * (i - 1) // 2 + j
        return ans
```

