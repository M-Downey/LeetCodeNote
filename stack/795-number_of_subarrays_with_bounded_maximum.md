### [795. 区间子数组个数](https://leetcode.cn/problems/number-of-subarrays-with-bounded-maximum/)

- 中等

给你一个整数数组 `nums` 和两个整数：`left` 及 `right` 。找出 `nums` 中连续、非空且其中最大元素在范围 `[left, right]` 内的子数组，并返回满足条件的子数组的个数。

生成的测试用例保证结果符合 **32-bit** 整数范围。

**示例 1：**

```
输入：nums = [2,1,4,3], left = 2, right = 3
输出：3
解释：满足条件的三个子数组：[2], [2, 1], [3]
```

**示例 2：**

```
输入：nums = [2,9,2,5,6], left = 2, right = 8
输出：7
```

**提示：**

- `1 <= nums.length <= 10^5`
- `0 <= nums[i] <= 10^9`
- `0 <= left <= right <= 10^9`

**解法一：单调栈**

```python
class Solution:
    def numSubarrayBoundedMax(self, nums: List[int], left: int, right: int) -> int:
        # 单调栈，考虑每个元素 nums[i] 的贡献
        # 以 nums[i] 为最大值的子数组的个数
        # l[i] 表示左边第一个更大的值的索引
        # r[i] 表示右边第一个更大的值的索引
        n = len(nums)
        l, r = [-1] * n, [n] * n
        stack = []
        for i in range(n - 1, -1, -1):
            # 遇到一个比栈顶元素更大的，就弹出栈顶元素比为它赋值
            while stack and nums[i] > nums[stack[-1]]:
                l[stack.pop()] = i
            stack.append(i)
        stack = []
        for i in range(n):
            # 如果是一段重复的数字，那么右边界就是右边第一个，避免重复
            while stack and nums[i] >= nums[stack[-1]]:
                r[stack.pop()] = i
            stack.append(i)
        # print(l)
        # print(r)
        ans = 0
        for i in range(n):
            if nums[i] >= left and nums[i] <= right:
                ans += (i - l[i]) * (r[i] - i)
        return ans
```

**解法二：一次遍历**

```python
class Solution:
    def numSubarrayBoundedMax(self, nums: List[int], left: int, right: int) -> int:
        # 考虑以 nums[i] 为结尾的所有子数组
        # 如果 nums[i] > right，就不考虑以它为结尾的子数组了
        # 如果 left <= nums[i] <= right，那么计算它和前面的 2 的差距，就是子数组的个数
        # 如果 nums[i] < left，如果它前面有元素在[left, right]之间，那么就加上 last1-last2
        ans = 0
        last1, last2 = -1, -1
        n = len(nums)
        for i in range(n):
            if left <= nums[i] <= right:
                last1 = i
            elif nums[i] > right:
                last2 = i
                # 在遇到新的 [left, right] 之前，遇到的0不计数
                last1 = -1
            # 如果在遇到2之后 遇到了1
            if last1 != -1:
                ans += last1 - last2
        return ans
```

