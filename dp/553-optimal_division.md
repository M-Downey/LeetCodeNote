### [553. 最优除法](https://leetcode.cn/problems/optimal-division/)

- 中等

给定一组**正整数，**相邻的整数之间将会进行浮点除法操作。例如， [2,3,4] -> 2 / 3 / 4 。

但是，你可以在任意位置添加任意数目的括号，来改变算数的优先级。你需要找出怎么添加括号，才能得到**最大的**结果，并且返回相应的字符串格式的表达式。**你的表达式不应该含有冗余的括号。**

**示例：**

```
输入: [1000,100,10,2]
输出: "1000/(100/10/2)"
解释:
1000/(100/10/2) = 1000/((100/10)/2) = 200
但是，以下加粗的括号 "1000/((100/10)/2)" 是冗余的，
因为他们并不影响操作的优先级，所以你需要返回 "1000/(100/10/2)"。

其他用例:
1000/(100/10)/2 = 50
1000/(100/(10/2)) = 50
1000/100/10/2 = 0.5
1000/100/(10/2) = 2
```

**说明:**

1. 输入数组的长度在 [1, 10] 之间。
2. 数组中每个元素的大小都在 [2, 1000] 之间。
3. 每个测试用例只有一个最优除法解。

**解法一：DP**

```python
class Node:
    def __init__(self):
        self.minVal = 1e4
        self.maxVal = 0
        self.minStr = ""
        self.maxStr = ""

class Solution:
    def optimalDivision(self, nums: List[int]) -> str:
        # dp[i][j] 表示 nums[i: j + 1] 的最大值最小值以及构成其值的字符串
        # dp[i][i] 等于本身，字符串也等于本身
        # dp[i][j] = dp[i][k], dp[k + 1][j] for k in range(i, j)
        # 考虑最小值，如果左边最小除以右边最大，更小，就更新，字符串更新为左+右（如果k不是j-1，就要加括号）
        # 考虑最大值，如果左边最大除以右边最小，更大，就更新，字符串更新为左+右（如果k不是j-1，就要加括号）
        # 从倒数第二行开始赋值
        n = len(nums)
        dp = [[Node() for _ in range(n)] for _ in range(n)]
        for i, num in enumerate(nums):
            dp[i][i].minVal = num
            dp[i][i].maxVal = num
            dp[i][i].minStr = str(num)
            dp[i][i].maxStr = str(num)
        for i in range(n - 2, -1, -1):
            for j in range(i + 1, n):
                for k in range(i, j):
                    if dp[i][j].maxVal < dp[i][k].maxVal / dp[k + 1][j].minVal:
                        dp[i][j].maxVal = dp[i][k].maxVal / dp[k + 1][j].minVal
                        if k + 1 == j:
                            dp[i][j].maxStr = dp[i][k].maxStr + '/' + dp[k + 1][j].minStr
                        else:
                            dp[i][j].maxStr = dp[i][k].maxStr + '/(' + dp[k + 1][j].minStr + ')'
                    if dp[i][j].minVal > dp[i][k].minVal / dp[k + 1][j].maxVal:
                        dp[i][j].minVal = dp[i][k].minVal / dp[k + 1][j].maxVal
                        if k + 1 == j:
                            dp[i][j].minStr = dp[i][k].minStr + '/' + dp[k + 1][j].maxStr
                        else:
                            dp[i][j].minStr = dp[i][k].minStr + '/(' + dp[k + 1][j].maxStr + ')'
        return dp[0][n - 1].maxStr
```

**解法二：数学**

```python
class Solution:
    def optimalDivision(self, nums: List[int]) -> str:
        if len(nums) == 1:
            return str(nums[0])
        if len(nums) == 2:
            return str(nums[0]) + "/" + str(nums[1])
        # 一堆大于 1 的数相除能得到的最小的值，就是从左往右除
        # 如果先局部除了一下，那么少一个除数，而且新除数比2者中的被除数更小，结果就会更大
        # 比如这堆数中有 /10/5，本来是除以10，再除以5
        # 先算局部就是/(10/5)，就是除以2，结果会变大
        return str(nums[0]) + "/(" + "/".join(map(str, nums[1:])) + ")"
```

