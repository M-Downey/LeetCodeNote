### [739. 每日温度](https://leetcode.cn/problems/daily-temperatures/)

- 中等

给定一个整数数组 `temperatures` ，表示每天的温度，返回一个数组 `answer` ，其中 `answer[i]` 是指对于第`i` 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

**示例 1:**

```
输入: temperatures = [73,74,75,71,69,72,76,73]
输出: [1,1,4,2,1,1,0,0]
```

**示例 2:**

```
输入: temperatures = [30,40,50,60]
输出: [1,1,1,0]
```

**示例 3:**

```
输入: temperatures = [30,60,90]
输出: [1,1,0]
```

**提示：**

- `1 <= temperatures.length <= 105`
- `30 <= temperatures[i] <= 100`

**解法一：暴力双重循环**

会超时

**解法二：单调栈**

单调栈：栈中元素非增或非减

本题中，单调栈中存的是索引，是**温度非增序列**

如果**当前温度小于等于栈顶元素**，则入栈

如果**当前温度大于栈顶元素**，这天就是**栈顶元素右边温度更高的第一天**，计算天数即可，并且持续`pop`比较，直到栈空或者当前温度小于等于栈顶元素

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        # 单调栈，栈中存非增温度的索引
        n = len(temperatures)
        ans = [0]*n
        stack = [0]
        for i in range(1, n):
            if temperatures[i] <= temperatures[stack[-1]]:
                stack.append(i)
            else:
                while stack and temperatures[i] > temperatures[stack[-1]]:
                    ans[stack[-1]] = i-stack[-1]
                    stack.pop()
                stack.append(i)
        return ans
```

