### [1235. 规划兼职工作](https://leetcode.cn/problems/maximum-profit-in-job-scheduling/)

- 困难

你打算利用空闲时间来做兼职工作赚些零花钱。

这里有 `n` 份兼职工作，每份工作预计从 `startTime[i]` 开始到 `endTime[i]` 结束，报酬为 `profit[i]`。

给你一份兼职工作表，包含开始时间 `startTime`，结束时间 `endTime` 和预计报酬 `profit` 三个数组，请你计算并返回可以获得的最大报酬。

注意，时间上出现重叠的 2 份工作不能同时进行。

如果你选择的工作在时间 `X` 结束，那么你可以立刻进行在时间 `X` 开始的下一份工作。

**示例 1：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/19/sample1_1584.png)

```
输入：startTime = [1,2,3,3], endTime = [3,4,5,6], profit = [50,10,40,70]
输出：120
解释：
我们选出第 1 份和第 4 份工作， 
时间范围是 [1-3]+[3-6]，共获得报酬 120 = 50 + 70。
```

**示例 2：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/19/sample22_1584.png)

```
输入：startTime = [1,2,3,4,6], endTime = [3,5,10,6,9], profit = [20,20,100,70,60]
输出：150
解释：
我们选择第 1，4，5 份工作。 
共获得报酬 150 = 20 + 70 + 60。
```

**示例 3：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/19/sample3_1584.png)

```
输入：startTime = [1,1,1], endTime = [2,3,4], profit = [5,6,4]
输出：6
```

**提示：**

- `1 <= startTime.length == endTime.length == profit.length <= 5 * 10^4`
- `1 <= startTime[i] < endTime[i] <= 10^9`
- `1 <= profit[i] <= 10^4`

**解法一：DP+二分查找**

[三叶题解](https://leetcode.cn/problems/maximum-profit-in-job-scheduling/solution/by-ac_oier-rgup/)

[二分法模板](https://blog.csdn.net/qq_41221520/article/details/108277801)

```python
class Solution:
    def jobScheduling(self, startTime: List[int], endTime: List[int], profit: List[int]) -> int:
        # dp[i] 代表前 i 个 job 能获得的最大利润
        # dp[1] = profit[0]
        # 考虑第 i 个工作，可以做可以不做
        # 做的情况又有2种，只做这个，还是接在某个任务后
        # 能在它之前做的任务，要满足结束时间 <= 任务 i 的开始时间
        # 为了保证所有能被接的任务都在任务 i 之前，按照任务的结束时间排序
        # 这样就可以用二分查找，找到小于等于任务 i 开始时间的最大结束时间的任务j 则 [1, j]任务都可以被接，比较就可以了(事实上不用比较)因为dp[j]相对于更靠前的dp[k]，一定会更赚钱，所以只考虑dp[j]即可
        n = len(startTime)
        jobs = [(startTime[i], endTime[i], profit[i]) for i in range(n)]
        jobs.sort(key=lambda x: x[1])
        # print(jobs)
        dp = [0] * (n + 1)
        for i in range(1, n + 1):
            st, et, pt = jobs[i - 1]
            # 不做第 i 个和只做第 i 个
            dp[i] = max(dp[i - 1], pt)
            # 找小于等于第 i 个开始时间的最大结束时间任务 j
            left, right = 0, i - 1
            while left <= right:
                mid = (left + right) // 2
                # 只要当前时间大于，就一定要往左走
                if jobs[mid][1] > st:
                    right = mid - 1
                else: # 不管是小于，还是等于，都要往右走
                    left = mid + 1
            if right >= 0 and jobs[right][1] <= st:
                dp[i] = max(dp[i], dp[right + 1] + pt)
        # print(dp)
        return dp[n]
```

