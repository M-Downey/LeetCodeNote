### [27. 移除元素](https://leetcode.cn/problems/remove-element/)

- 简单

给你一个数组 `nums` 和一个值 `val`，你需要 **[原地](https://baike.baidu.com/item/原地算法)** 移除所有数值等于 `val` 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 `O(1)` 额外空间并 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组**。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

**说明:**

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以**「引用」**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```c
// nums 是以“引用”方式传递的。也就是说，不对实参作任何拷贝
int len = removeElement(nums, val);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

**示例 1：**

```
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2]
解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。
```

**示例 2：**

```
输入：nums = [0,1,2,2,3,0,4,2], val = 2
输出：5, nums = [0,1,4,0,3]
解释：函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。注意这五个元素可为任意顺序。你不需要考虑数组中超出新长度后面的元素。
```

**提示：**

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`

**解法一：双重循环暴力法**

遍历数组，如果一个`nums[i] == value`那么移动`i`后面的所有元素

**注意：**删除一个元素后，`i`和数组长度`n`要自减`1`

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        n = len(nums)
        i = 0
        while i < n:
            if nums[i] == val:
                for j in range(i+1, n):
                    nums[j-1] = nums[j]
                i -= 1
                n -= 1
            i += 1
        return n
```

**解法二：**

排序+遍历删除

**解法三：快慢双指针**

`fast`和`slow`指针一起遍历数组，`slow`指向输出数组，`fast`遍历数组，如果`fast`指向的不是`val`说明它要输出，就给`slow`赋值，并一起自增

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        n = len(nums)
        slow, fast = 0, 0

        while fast < n:
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
            fast += 1
        return slow
```

**解法四：首尾双指针（解法三的优化）**

如果`nums[left] == val`则把`nums[right]`赋过来并把`right--`，否则`left++`

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        left, right = 0, len(nums)-1
        # [2] val=2
        while left <= right:
            if nums[left] == val:
                nums[left] = nums[right]
                right -= 1
            else:
                left += 1
        return left
```

**解法五：累计移动**

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int count = 0;
        for(int i = 0; i < nums.length; i++){
            if(nums[i] == val){
                count++;
            }else{
                nums[i - count] = nums[i];
            }
        }
        return nums.length - count;
    }
}
```

