### [220. 存在重复元素 III](https://leetcode.cn/problems/contains-duplicate-iii/)

- 困难

给你一个整数数组 `nums` 和两个整数 `k` 和 `t` 。请你判断是否存在 **两个不同下标** `i` 和 `j`，使得 `abs(nums[i] - nums[j]) <= t` ，同时又满足 `abs(i - j) <= k` 。

如果存在则返回 `true`，不存在返回 `false`。

**示例 1：**

```
输入：nums = [1,2,3,1], k = 3, t = 0
输出：true
```

**示例 2：**

```
输入：nums = [1,0,1,1], k = 1, t = 2
输出：true
```

**示例 3：**

```
输入：nums = [1,5,9,1,5,9], k = 2, t = 3
输出：false
```

**提示：**

- `0 <= nums.length <= 2 * 10^4`
- `-2^31 <= nums[i] <= 2^31 - 1`

- `0 <= k <= 10^4`
- `0 <= t <= 2^31 - 1`

**解法一：滑动窗口**

要找到当前元素左边在窗口中的，小于等于它的最大值和大于等于它的最小值

并计算和它们的距离是否小于等于 `t`

顺序插入模块

[`bisect`](https://blog.csdn.net/w1301100424/article/details/99200842)

```python
from sortedcontainers import SortedList

class Solution:
    def containsNearbyAlmostDuplicate(self, nums: List[int], k: int, t: int) -> bool:
        # O(N logk)
        window = SortedList()
        for i in range(len(nums)):
            # len(window) == k
            if i > k:
                window.remove(nums[i - (k + 1)])
            window.add(nums[i])
            # 返回插入索引
            idx = bisect.bisect_left(window, nums[i])
            if idx > 0 and abs(window[idx] - window[idx-1]) <= t:
                return True
            if idx < len(window) - 1 and abs(window[idx+1] - window[idx]) <= t:
                return True
        return False
```

