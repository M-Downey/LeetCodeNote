### [436. 寻找右区间](https://leetcode.cn/problems/find-right-interval/)

- 中等

给你一个区间数组 `intervals` ，其中 `intervals[i] = [starti, endi]` ，且每个 `starti` 都 **不同** 。

区间 `i` 的 **右侧区间** 可以记作区间 `j` ，并满足 `startj`` >= endi` ，且 `startj` **最小化** 。

返回一个由每个区间 `i` 的 **右侧区间** 在 `intervals` 中对应下标组成的数组。如果某个区间 `i` 不存在对应的 **右侧区间** ，则下标 `i` 处的值设为 `-1` 。

**示例 1：**

```
输入：intervals = [[1,2]]
输出：[-1]
解释：集合中只有一个区间，所以输出-1。
```

**示例 2：**

```
输入：intervals = [[3,4],[2,3],[1,2]]
输出：[-1,0,1]
解释：对于 [3,4] ，没有满足条件的“右侧”区间。
对于 [2,3] ，区间[3,4]具有最小的“右”起点;
对于 [1,2] ，区间[2,3]具有最小的“右”起点。
```

**示例 3：**

```
输入：intervals = [[1,4],[2,3],[3,4]]
输出：[-1,2,-1]
解释：对于区间 [1,4] 和 [3,4] ，没有满足条件的“右侧”区间。
对于 [2,3] ，区间 [3,4] 有最小的“右”起点。
```

**提示：**

- `1 <= intervals.length <= 2 * 10^4`
- `intervals[i].length == 2`
- `-10^6 <= starti <= endi <= 10^6`
- 每个间隔的起点都 **不相同**

**解法一：二分查找**

将每个区间按照 `(idx, left)` 的格式，以区间开始值升序排列

这样对每个区间来说，只需要找第一个大于等于 `intervals[i][1]` 的位置即可，在上面的数组中进行二分查找

```python
class Solution:
    def findRightInterval(self, intervals: List[List[int]]) -> List[int]:
        n = len(intervals)
        left_idx = [(i, intervals[i][0]) for i in range(n)]
        left_idx.sort(key=lambda x: x[1])
        # print(left_idx)
        ans = [-1] * n
        for i in range(n):
            left, right = 0, n
            # 找第一个大于等于 intervals[i][1] 的元素
            while left < right:
                mid = (left + right) // 2
                if left_idx[mid][1] < intervals[i][1]:
                    left = mid + 1
                else:
                    right = mid
            # print(left)
            if left < n:
                ans[i] = left_idx[left][0]
        return ans
```

