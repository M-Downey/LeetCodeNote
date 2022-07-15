### [1. 两数之和](https://leetcode.cn/problems/two-sum/)

- 简单

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3	：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

**提示：**

- `2 <= nums.length <= 10^4`
- `-10^9 <= nums[i] <= 10^9`(很大，不能用数组了)
- `-10^9 <= target <= 10^9`
- **只会存在一个有效答案**

**解法一：**`HashMap`

本题呢，则要使用`map`，那么来看一下使用`array`和`set`来做哈希法的局限。

- 数组的大小是受限制的，而且如果元素很少，而哈希值太大会造成内存空间的浪费。
- `set`是一个集合，里面放的元素只能是一个`key`，而两数之和这道题目，不仅要判断`y`是否存在而且还要记录`y`的下标位置，因为要返回`x` 和 `y`的下标。所以`set `也不能用。

此时就要选择另一种数据结构：map ，map是一种key value的存储结构，可以用key保存数值，用value在保存数值所在的下标。

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        h = {}
        for idx, value in enumerate(nums):
            if target - nums[idx] in h:
                return [idx, h[target - nums[idx]]]
            else:
                h[value] = idx
```

