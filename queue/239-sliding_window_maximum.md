### [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

- 困难

[代码随想录](https://programmercarl.com/0239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.html)

给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

返回 *滑动窗口中的最大值* 。

**示例 1：**

```
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**示例 2：**

```
输入：nums = [1], k = 1
输出：[1]
```

**提示：**

- `1 <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`
- `1 <= k <= nums.length`

**解法一：暴力**

会超时

**解法二：单调队列**

对每个窗口维护一个单调递减的队列

单调队列：

不关注所有元素的顺序，只关心可能的最大值，所以对于每个元素来说，在队列中从后往前比较，如果大于队尾元素，就`pop`，最后把该元素加到队列中

 ![239.滑动窗口最大值](https://code-thinking.cdn.bcebos.com/gifs/239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.gif)

 ![239.滑动窗口最大值-2](https://code-thinking.cdn.bcebos.com/gifs/239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC-2.gif)

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        # 单调队列，q中存的是索引
        q = collections.deque()
        # 第一个窗口
        for i in range(k):
            while q and nums[i] >= nums[q[-1]]:
                q.pop()
            q.append(i)
        # 添加第一个窗口的最大值
        ans = [nums[q[0]]]

        for i in range(k, n):
            # q.push(nums[i])
            while q and nums[i] >= nums[q[-1]]:
                q.pop()
            q.append(i)
            # q.pop(nums[i])
            # 如果队首索引是在当前窗口之前，就要删除
            while q[0] <= i - k:
                q.popleft()
            ans.append(nums[q[0]])
        
        return ans
```

