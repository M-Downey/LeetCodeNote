### [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

- 简单

假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**示例 1：**

```
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶
```

**示例 2：**

```
输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。
1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```

**提示：**

- `1 <= n <= 45`

**解法一：**

`dp[i]`记录上第`n`阶楼梯的方法，则可以由`dp[i-1]`和`dp[i-2]`求出

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        dp = [0] * 46
        dp[1] = 1
        dp[2] = 2
        for i in range(3, 46):
            dp[i] = dp[i-1] + dp[i-2]
        return dp[n]
```

空间复杂度是`O(N)`了，可以用滚动数组变成`O(1)`

java 空间优化：

```java
class Solution {
    public int climbStairs(int n) {
        // dp[0] = 1, dp[1] = 1, dp[2] = 2
        // dp[i] = dp[i - 1] + dp[i - 2]
        int p0 = 1;
        int p1 = 1;
        for (int i = 2; i <= n; i++) {
            int p2 = p0 + p1;
            p0 = p1;
            p1 = p2;
        }
        return p1;
    }
}
```

