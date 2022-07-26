### [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)

- 中等

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回其中出现频率前 `k` 高的元素。你可以按 **任意顺序** 返回答案。

**示例 1:**

```
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
```

**示例 2:**

```
输入: nums = [1], k = 1
输出: [1]
```

**提示：**

- `1 <= nums.length <= 105`
- `k` 的取值范围是 `[1, 数组中不相同的元素的个数]`

- 题目数据保证答案唯一，换句话说，数组中前 `k` 个高频元素的集合是唯一的

**进阶：**你所设计算法的时间复杂度 **必须** 优于 `O(n log n)` ，其中 `n` 是数组大小。

**解法一：哈希表+排序**

哈希表构建数字，频率映射，然后按频率排序取前`k`个

```python
from collections import defaultdict

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        hm = defaultdict(int)
        for num in nums:
            hm[num] += 1
        
        ls = sorted(hm.items(), key=lambda x:x[1], reverse=True)[:k]
        print(ls)
        ans = []
        for item in ls:
            ans.append(item[0])
        return ans
```

**解法二：哈希表+优先队列（小根堆）**

法一的时间复杂度是`O(NlogN)`

为求前`k`个频率最大的数字，不需排序`N`个元素，只需维护一个长度为`k`的优先队列（有序队列）就行了

优先队列底层实现是堆

遍历哈希表，将`(frequency, num)`数对添加到优先队列中，如果队列长度大于`k`，就`pop`出最小的元素，最终队列中剩下最大的前`k`个元素

```python
from collections import defaultdict
import heapq
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        hm = defaultdict(int)
        for num in nums:
            hm[num] += 1
        
        # 优先队列/小根堆
        pri_q = []
        for key, freq in hm.items():
            heapq.heappush(pri_q, (freq, key))
            # 堆中元素大于k就移除最小的
            if len(pri_q) > k:
                heapq.heappop(pri_q)
        ans = [0]*k
        for i in range(k-1, -1, -1):
            ans[i] = heapq.heappop(pri_q)[1]
        return ans
```

