### [1760. 袋子里最少数目的球](https://leetcode.cn/problems/minimum-limit-of-balls-in-a-bag/)

- 中等

给你一个整数数组 `nums` ，其中 `nums[i]` 表示第 `i` 个袋子里球的数目。同时给你一个整数 `maxOperations` 。

你可以进行如下操作至多 `maxOperations` 次：

- 选择任意一个袋子，并将袋子里的球分到 **2** 个新的袋子中，每个袋子里都有 **正整数** 个球。
  - 比方说，一个袋子里有 `5` 个球，你可以把它们分到两个新袋子里，分别有 `1` 个和 `4` 个球，或者分别有 `2` 个和 `3` 个球。

你的开销是单个袋子里球数目的 **最大值** ，你想要 **最小化** 开销。

请你返回进行上述操作后的最小开销。

**示例 1：**

```
输入：nums = [9], maxOperations = 2
输出：3
解释：
- 将装有 9 个球的袋子分成装有 6 个和 3 个球的袋子。[9] -> [6,3] 。
- 将装有 6 个球的袋子分成装有 3 个和 3 个球的袋子。[6,3] -> [3,3,3] 。
装有最多球的袋子里装有 3 个球，所以开销为 3 并返回 3 。
```

**示例 2：**

```
输入：nums = [2,4,8,2], maxOperations = 4
输出：2
解释：
- 将装有 8 个球的袋子分成装有 4 个和 4 个球的袋子。[2,4,8,2] -> [2,4,4,4,2] 。
- 将装有 4 个球的袋子分成装有 2 个和 2 个球的袋子。[2,4,4,4,2] -> [2,2,2,4,4,2] 。
- 将装有 4 个球的袋子分成装有 2 个和 2 个球的袋子。[2,2,2,4,4,2] -> [2,2,2,2,2,4,2] 。
- 将装有 4 个球的袋子分成装有 2 个和 2 个球的袋子。[2,2,2,2,2,4,2] -> [2,2,2,2,2,2,2,2] 。
装有最多球的袋子里装有 2 个球，所以开销为 2 并返回 2 。
```

**示例 3：**

```
输入：nums = [7,17], maxOperations = 2
输出：7
```

**提示：**

- `1 <= nums.length <= 10^5`
- `1 <= maxOperations, nums[i] <= 10^9`

**解法一：二分搜索**

开销具有二段性，可以用二分法

开销取值在 1 和初始背包最大值之间

用二分法尝试每一个值 y

然后把每个背包都分到 `<= y`, 计算操作数， `(nums[i] - 1)/y` 下取整

- `1<= nums[i] <= y` 取0

- `y+1 <= nums[i] <= 2y` 取1

如果小于等于 `maxOperations` 就调整上界（还有更多的操作数，可以分更小）

否则，就调整下界

二分具体过程，根据循环条件，有2种写法：

```python
# 1. up <= down
# 要用额外变量 ans 记录答案
ans = 0
while down <= up:
	mid = (up + down) // 2
    op_nums = 0
    for i in range(n):
        op_nums += (nums[i] - 1) // mid
	if op_nums <= maxOperations:
		ans = mid
        up = mid - 1
	# 当前 mid 是不合法的，down必须是mid+1
	else:
		down = mid + 1

# 2. up < down
# 最后要的是 up 值，因为 up 值一直是合法的

while down < up:
	mid = (up + down) // 2
    op_nums = 0
    for i in range(n):
        op_nums += (nums[i] - 1) // mid
	if op_nums <= maxOperations:
		up = mid
	# 当前 mid 是不合法的，down必须是mid+1
	else:
		down = mid + 1
```

```python
class Solution:
    def minimumSize(self, nums: List[int], maxOperations: int) -> int:
        # 开销具有二段性，可以用二分法
        # 开销取值在 1 和初始背包最大值之间
        # 用二分法尝试每一个值 y
        # 然后把每个背包都分到 <= y, 计算操作数， (nums[i] - 1)/y 下取整
        # 1<= nums[i] <= y 取0
        # y+1 <= nums[i] <= 2y 取1
        # 如果小于等于 maxOperations 就调整上界（还有更多的操作数，可以分更小）
        # 否则，就调整下届
        n = len(nums)
        up, down = max(nums), 1
        # down = up 会无限循环
        while down < up:
            mid = (up + down) // 2
            op_nums = 0
            for i in range(n):
                op_nums += (nums[i] - 1) // mid
            if op_nums <= maxOperations:
                up = mid
            # 当前 mid 是不合法的，down必须是mid+1
            else:
                down = mid + 1
        return up
```

```python
class Solution:
    def minimumSize(self, nums: List[int], maxOperations: int) -> int:
        # 开销具有二段性，可以用二分法
        # 开销取值在 1 和初始背包最大值之间
        # 用二分法尝试每一个值 y
        # 然后把每个背包都分到 <= y, 计算操作数， (nums[i] - 1)/y 下取整
        # 1<= nums[i] <= y 取0
        # y+1 <= nums[i] <= 2y 取1
        # 如果小于等于 maxOperations 就调整上界（还有更多的操作数，可以分更小）
        # 否则，就调整下界
        n = len(nums)
        up, down = max(nums), 1
        ans = 0
        while down <= up:
            mid = (up + down) // 2
            op_nums = 0
            for i in range(n):
                op_nums += (nums[i] - 1) // mid
            if op_nums <= maxOperations:
                ans = mid
                up = mid - 1
            # 当前 mid 是不合法的，down必须是mid+1
            else:
                down = mid + 1
        return ans
```

