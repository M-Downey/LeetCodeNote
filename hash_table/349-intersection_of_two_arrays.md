### [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)

- 简单

给定两个数组 `nums1` 和 `nums2` ，返回 *它们的交集* 。输出结果中的每个元素一定是 **唯一** 的。我们可以 **不考虑输出结果的顺序** 。

**示例 1：**

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```

**示例 2：**

```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
解释：[4,9] 也是可通过的
```

**提示：**

- `1 <= nums1.length, nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 1000`

**解法一：**数组作为哈希表

因为本题数在1000以内，所以哈希表开销也不大

```python
# 数组作为HashTable
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        ht = [0] * 1001
        # 遍历数组1，有的数字就置1
        for num in nums1:
            ht[num] = 1
        res = []
        for num in nums2:
            if ht[num]:
                res.append(num)
                # 结果不重复，故再置0
                ht[num] = 0
        return res
```

```python
# 直接用set
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return list(set(nums1) & set(nums2))    # 两个数组先变成集合，求交集后还原为数组
```

