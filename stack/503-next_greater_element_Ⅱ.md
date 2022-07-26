### [503. 下一个更大元素 II](https://leetcode.cn/problems/next-greater-element-ii/)

- 中等

给定一个循环数组 `nums` （ `nums[nums.length - 1]` 的下一个元素是 `nums[0]` ），返回 *`nums` 中每个元素的 **下一个更大元素*** 。

数字 `x` 的 **下一个更大的元素** 是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 `-1` 。

**示例 1:**

```
输入: nums = [1,2,1]
输出: [2,-1,2]
解释: 第一个 1 的下一个更大的数是 2；
数字 2 找不到下一个更大的数； 
第二个 1 的下一个最大的数需要循环搜索，结果也是 2。
```

**示例 2:**

```
输入: nums = [1,2,3,4,3]
输出: [2,3,4,-1,4]
```

**提示:**

- `1 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`

**解法一：单调栈+拼接数组**

元素`nums[i]`的右边更大元素可能在`nums[i+1]`到`nums[n-1]`以及`nums[0]`到`nums[i-1]`，所以可以把`nums`倍长，后面再拼接一个`nums`

然后求`nums[:n]`的右边第一个更大元素即可

```python
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        n1 = len(nums)
        nums[n1:] = nums
        n = len(nums)

        ans = [-1]*n
        next_idxs = [0] * n
        stack = [0]
        for i in range(1, n):
            if nums[i] <= nums[stack[-1]]:
                stack.append(i)
            else:
                while stack and nums[i] > nums[stack[-1]]:
                    ans[stack[-1]] = nums[i]
                    stack.pop()
                stack.append(i)
        return ans[:n1]
```

**解法二：用模避免拼接数组**

```python
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        n = len(nums)
        ans = [-1]*n
        stack = [0]
        for i in range(1, 2*n):
            while stack and nums[i % n] > nums[stack[-1]]:
                ans[stack[-1]] = nums[i % n]
                stack.pop()
            stack.append(i % n)
        return ans
```

