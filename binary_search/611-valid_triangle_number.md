### [611. 有效三角形的个数](https://leetcode.cn/problems/valid-triangle-number/)

- 中等

给定一个包含非负整数的数组 `nums` ，返回其中可以组成三角形三条边的三元组个数。

**示例 1:**

```
输入: nums = [2,2,3,4]
输出: 3
解释:有效的组合是: 
2,3,4 (使用第一个 2)
2,3,4 (使用第二个 2)
2,2,3
```

**示例 2:**

```
输入: nums = [4,2,3,4]
输出: 4
```

**提示:**

- `1 <= nums.length <= 1000`
- `0 <= nums[i] <= 1000`

**解法一：排序+二分**

三角形的三边都要满足，两边之和大于第三边，将 `nums` 排序后

**只需满足两个小边之和大于第三边即可**

那么就可以双重循环遍历前两条边，再找第三边，累加计数即可

需要在 `nums[i+1: n]` 中，找到一个最大的 `k` 使得它是最后一个满足 `nums[i] + nums[j] > nums[k]` 的，也就是找到 `nums[i] + nums[j]` 的插入位置 `k` ， `k` 代表了有它前面有多少个元素，还需要减去第一个元素 `nums[j]` ，才是第三边的个数

```python
class Solution:
    def triangleNumber(self, nums: List[int]) -> int:
        # 排序后，只需验证 a+b>c 的个数
        nums.sort()
        ans = 0
        n = len(nums)
        # 在 nums[j+1: n] 中找最大的 k 满足 nums[i]+nums[j] > nums[k]
        for i in range(n):
            for j in range(i + 1, n):
                k = bisect.bisect_left(nums[j: n], nums[i] + nums[j])
                # 不算第 j 位的，因为此时三边是 a b b
                ans += max(0, k - 1)
        return ans
```

**解法二：优化**

可以看到在找满足 `nums[i] + nums[j] > nums[k]` 时，只要 `j` 增大，`k` 也会增大，所以只需二重循环即可

```python
class Solution:
    def triangleNumber(self, nums: List[int]) -> int:
        # 排序后，只需验证 a+b>c 的个数
        nums.sort()
        ans = 0
        n = len(nums)
        # 在 nums[j+1: n] 中找最大的 k 满足 nums[i]+nums[j] > nums[k]
        for i in range(n):
            k = i + 2
            for j in range(i + 1, n):
                while k < n and nums[i] + nums[j] > nums[k]:
                    k += 1
                ans +=  max(k - j - 1, 0)
        return ans
```

