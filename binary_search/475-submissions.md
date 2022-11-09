### [475. 供暖器](https://leetcode.cn/problems/heaters/)

- 中等

冬季已经来临。 你的任务是设计一个有固定加热半径的供暖器向所有房屋供暖。

在加热器的加热半径范围内的每个房屋都可以获得供暖。

现在，给出位于一条水平线上的房屋 `houses` 和供暖器 `heaters` 的位置，请你找出并返回可以覆盖所有房屋的最小加热半径。

**说明**：所有供暖器都遵循你的半径标准，加热的半径也一样。

**示例 1:**

```
输入: houses = [1,2,3], heaters = [2]
输出: 1
解释: 仅在位置2上有一个供暖器。如果我们将加热半径设为1，那么所有房屋就都能得到供暖。
```

**示例 2:**

```
输入: houses = [1,2,3,4], heaters = [1,4]
输出: 1
解释: 在位置1, 4上有两个供暖器。我们需要将加热半径设为1，这样所有房屋就都能得到供暖。
```

**示例 3:**

```
输入：houses = [1,5], heaters = [2]
输出：3
```

**提示：**

- `1 <= houses.length, heaters.length <= 3 * 10^4`
- `1 <= houses[i], heaters[i] <= 10^9`

**解法一：二分**

- 遍历房子，找到每个房子左右两边的加热器，计算其中的最小值

- 找到所有房子离最近的加热器的最大值
- 注意，房子位于最左边和最右边的位置，只能找到一个加热器，要单独处理这种情况
- 加热器数组要先排序

```python
class Solution:
    def findRadius(self, houses: List[int], heaters: List[int]) -> int:
        # house[i] 是房子位置, heaters[i] 是某个加热器的位置
        # 对于每个房子来说，要找离它最近的加热器，取差值
        # 最后取所有差值中的最大值
        ans = 0
        heaters.sort()
        n = len(houses)
        for i in range(n):
            idx = bisect.bisect_left(heaters, houses[i])
            if idx > 0 and idx < len(heaters):
                # print(i, idx)
                tmp = min(abs(heaters[idx] - houses[i]), abs(heaters[idx - 1] - houses[i]))
            elif idx == 0:
                tmp = abs(heaters[idx] - houses[i])
            elif idx == len(heaters):
                tmp = abs(heaters[idx - 1] - houses[i])
            ans = max(ans, tmp)
        return ans
```

