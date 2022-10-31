### [901. 股票价格跨度](https://leetcode.cn/problems/online-stock-span/)

- 中等

编写一个 `StockSpanner` 类，它收集某些股票的每日报价，并返回该股票当日价格的跨度。

今天股票价格的跨度被定义为股票价格小于或等于今天价格的最大连续日数（从今天开始往回数，包括今天）。

例如，如果未来7天股票的价格是 `[100, 80, 60, 70, 60, 75, 85]`，那么股票跨度将是 `[1, 1, 1, 2, 1, 4, 6]`。

**示例：**

```
输入：["StockSpanner","next","next","next","next","next","next","next"], [[],[100],[80],[60],[70],[60],[75],[85]]
输出：[null,1,1,1,2,1,4,6]
解释：
首先，初始化 S = StockSpanner()，然后：
S.next(100) 被调用并返回 1，
S.next(80) 被调用并返回 1，
S.next(60) 被调用并返回 1，
S.next(70) 被调用并返回 2，
S.next(60) 被调用并返回 1，
S.next(75) 被调用并返回 4，
S.next(85) 被调用并返回 6。

注意 (例如) S.next(75) 返回 4，因为截至今天的最后 4 个价格
(包括今天的价格 75) 小于或等于今天的价格。
```

**提示：**

1. 调用 `StockSpanner.next(int price)` 时，将有 `1 <= price <= 10^5`。
2. 每个测试用例最多可以调用 `10000` 次 `StockSpanner.next`。
3. 在所有测试用例中，最多调用 `150000` 次 `StockSpanner.next`。
4. 此问题的总时间限制减少了 50%。

**解法一：单调栈**

要求股价小于等于今日价格的最大连续日数，就是要找**今日价格之前的第一个更大的价格**，可以用单调递减栈存储股票价格（同时存储股票日期），每次入栈时，检查栈顶的元素，如果小于等于今日价格，就出栈

```python
class StockSpanner:

    def __init__(self):
        self.prices = [(-1, inf)]
        self.idx = -1

    # 单调栈，栈中只存放递减的价格序列
    def next(self, price: int) -> int:
        self.idx += 1
        while price >= self.prices[-1][1]:
            self.prices.pop()
        self.prices.append((self.idx, price))
        return self.idx - self.prices[-2][0]

# Your StockSpanner object will be instantiated and called as such:
# obj = StockSpanner()
# param_1 = obj.next(price)
```

三叶版实现

```python
class StockSpanner:
    # 找到每个元素左边第一个更大的元素
    def __init__(self):
        # 用(idx, price)存储股票
        self.stack = []
        # 当前位置
        self.idx = 0

    def next(self, price: int) -> int:
        ans = 1
        # 只要当前价格更大就一直出栈
        while self.stack and price >= self.stack[-1][1]:
            self.stack.pop()
        prev = -1 if not self.stack else self.stack[-1][0]
        ans = self.idx - prev
        self.stack.append((self.idx, price))
        self.idx += 1
        return ans
# Your StockSpanner object will be instantiated and called as such:
# obj = StockSpanner()
# param_1 = obj.next(price)
```

