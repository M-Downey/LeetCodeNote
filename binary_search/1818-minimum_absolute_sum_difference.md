### [1818. 绝对差值和](https://leetcode.cn/problems/minimum-absolute-sum-difference/)

- 中等

给你两个正整数数组 `nums1` 和 `nums2` ，数组的长度都是 `n` 。

数组 `nums1` 和 `nums2` 的 **绝对差值和** 定义为所有 `|nums1[i] - nums2[i]|`（`0 <= i < n`）的 **总和**（**下标从 0 开始**）。

你可以选用 `nums1` 中的 **任意一个** 元素来替换 `nums1` 中的 **至多** 一个元素，以 **最小化** 绝对差值和。

在替换数组 `nums1` 中最多一个元素 **之后** ，返回最小绝对差值和。因为答案可能很大，所以需要对 `109 + 7` **取余** 后返回。

`|x|` 定义为：

- 如果 `x >= 0` ，值为 `x` ，或者
- 如果 `x <= 0` ，值为 `-x`

**示例 1：**

```
输入：nums1 = [1,7,5], nums2 = [2,3,5]
输出：3
解释：有两种可能的最优方案：
- 将第二个元素替换为第一个元素：[1,7,5] => [1,1,5] ，或者
- 将第二个元素替换为第三个元素：[1,7,5] => [1,5,5]
两种方案的绝对差值和都是 |1-2| + (|1-3| 或者 |5-3|) + |5-5| = 3
```

**示例 2：**

```
输入：nums1 = [2,4,6,8,10], nums2 = [2,4,6,8,10]
输出：0
解释：nums1 和 nums2 相等，所以不用替换元素。绝对差值和为 0
```

**示例 3：**

```
输入：nums1 = [1,10,4,4,2,7], nums2 = [9,3,5,1,7,4]
输出：20
解释：将第一个元素替换为第二个元素：[1,10,4,4,2,7] => [10,10,4,4,2,7]
绝对差值和为 |10-9| + |10-3| + |4-5| + |4-1| + |2-7| + |7-4| = 20
```

**提示：**

- `n == nums1.length`
- `n == nums2.length`
- `1 <= n <= 10^5`
- `1 <= nums1[i], nums2[i] <= 10^5`

**解法一：排序+二分**

1. 思路

先求出差的绝对值之和，要想更改一个元素，使它最小

也就是对于每个 `nums2[i]` 来说，要找它在 `nums1` 中最接近的数，并且计算它们差值的最小值

对于答案的贡献由 `nums1[i] - nums1[i]` 变为 `nums1[j] - nums2[i]` ，这个差值要最大

2. 细节

用二分查找库：`bisect.bisect_left(array, num)`

找到 `num` 在 `array` 中插入位置的左边界，即找到 `array` 中第一个大于等于 `num` 的位置

实现的时候需要检查 `idx` 和 `idx-1` 的位置，因为可能是左边最接近也可能是右边最接近

```python
class Solution:
    def minAbsoluteSumDiff(self, nums1: List[int], nums2: List[int]) -> int:
        n = len(nums1)
        total = sum(abs(x - y) for x, y in zip(nums1, nums2))
        a = sorted(nums1)
        best = 0
        # 计算每个 nums2[j] 与最接近的 nums1[i] 的差（取最小），从而确定total中最多能降低多少
        for x, y in zip(nums1, nums2):
            cur = abs(x - y)
            # a[idx] 是 >= y 的第一个数，要检查它以及它前面的数，和y的差距
            idx = bisect.bisect_left(a, y)
            if idx < n:
                best = max(best, cur - (a[idx] - y))
            if idx > 0:
                best = max(best, cur - (y - a[idx - 1]))
        
        return (total - best) % (10**9 + 7)
```

