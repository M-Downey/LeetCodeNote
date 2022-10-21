#### [219. 存在重复元素 II](https://leetcode.cn/problems/contains-duplicate-ii/)

- 简单

给你一个整数数组 `nums` 和一个整数 `k` ，判断数组中是否存在两个 **不同的索引** `i` 和 `j` ，满足 `nums[i] == nums[j]` 且 `abs(i - j) <= k` 。如果存在，返回 `true` ；否则，返回 `false` 。

**示例 1：**

```
输入：nums = [1,2,3,1], k = 3
输出：true
```

**示例 2：**

```
输入：nums = [1,0,1,1], k = 1
输出：true
```

**示例 3：**

```
输入：nums = [1,2,3,1,2,3], k = 2
输出：false
```

**提示：**

- `1 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`
- `0 <= k <= 10^5`

**解法一：滑动窗口**

看成一个长为 `k+1` 的滑动窗口，窗口中如果有相等的元素（用哈希表判断）就返回 `True`

遍历结束后，没找到相等的元素，就返回 `False`

```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        if k == 0:
            return False
        n = len(nums)
        left = 0
        window = set([nums[0]])
        for right in range(1, n):
            # right - left + 1 > k + 1
            while right - left > k:
                window.remove(nums[left])
                left += 1
            if nums[right] in window:
                return True
            window.add(nums[right])
        return False
```

