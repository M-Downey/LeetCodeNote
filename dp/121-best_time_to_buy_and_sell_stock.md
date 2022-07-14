### [121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

- 简单

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

**示例 1：**

```
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

**示例 2：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

**提示：**

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`

**解法一：**暴力法

我们需要找出给定数组中两个数字之间的最大差值（即，最大利润）。此外，第二个数字（卖出价格）必须大于第一个数字（买入价格）。

形式上，对于每组 `i` 和 `j`（其中 `j > i`）我们需要找出 
$$
\max(prices[j] - prices[i])
$$

```python
# python此方法会超时
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        ans = 0
        for i in range(len(prices)):
            for j in range(i + 1, len(prices)):
                ans = max(ans, prices[j] - prices[i])
        return ans
```

**解法二：**一次遍历

考虑：在第`i(i>=2)`天卖出股票，最多能挣多少钱

只需考虑`i`之前的最低价格和上一个`maxProfit`，取两者大值为新的`maxProfit`

我们来假设自己来购买股票。随着时间的推移，每天我们都可以选择出售股票与否。那么，假设在第 `i` 天，如果我们要在今天卖股票，那么我们能赚多少钱呢？

显然，如果我们真的在买卖股票，我们肯定会想：如果我是在历史最低点买的股票就好了！太好了，在题目中，我们只要用一个变量记录一个历史最低价格 `minprice`，我们就可以假设自己的股票是在那天买的。那么我们在第 `i` 天卖出股票能得到的利润就是 `prices[i] - minprice`。

因此，我们只需要遍历价格数组一遍，记录历史最低点，然后在每一天考虑这么一个问题：如果我是在历史最低点买进的，那么我今天卖出能赚多少钱？当考虑完所有天数之时，我们就得到了最好的答案。

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # 假设在第i天出股票，并记录i天之前，股票最低价，那么最大利润是price[i] - min
        minPrice = prices[0]
        maxPro = 0
        for i in range(1, len(prices)):
            maxPro = max(maxPro, prices[i] - minPrice)
            if prices[i] < minPrice:
                minPrice = prices[i]
        return maxPro
```

