### [1774. 最接近目标价格的甜点成本](https://leetcode.cn/problems/closest-dessert-cost/)

- 中等

你打算做甜点，现在需要购买配料。目前共有 `n` 种冰激凌基料和 `m` 种配料可供选购。而制作甜点需要遵循以下几条规则：

- 必须选择 **一种** 冰激凌基料。
- 可以添加 **一种或多种** 配料，也可以不添加任何配料。
- 每种类型的配料 **最多两份** 。

给你以下三个输入：

- `baseCosts` ，一个长度为 `n` 的整数数组，其中每个 `baseCosts[i]` 表示第 `i` 种冰激凌基料的价格。

- `toppingCosts`，一个长度为 `m` 的整数数组，其中每个 `toppingCosts[i]` 表示 **一份** 第 `i` 种冰激凌配料的价格。
- `target` ，一个整数，表示你制作甜点的目标价格。

你希望自己做的甜点总成本尽可能接近目标价格 `target` 。

返回最接近 `target` 的甜点成本。如果有多种方案，返回 **成本相对较低** 的一种。

**示例 1：**

```
输入：baseCosts = [1,7], toppingCosts = [3,4], target = 10
输出：10
解释：考虑下面的方案组合（所有下标均从 0 开始）：
- 选择 1 号基料：成本 7
- 选择 1 份 0 号配料：成本 1 x 3 = 3
- 选择 0 份 1 号配料：成本 0 x 4 = 0
总成本：7 + 3 + 0 = 10 。
```

**示例 2：**

```
输入：baseCosts = [2,3], toppingCosts = [4,5,100], target = 18
输出：17
解释：考虑下面的方案组合（所有下标均从 0 开始）：
- 选择 1 号基料：成本 3
- 选择 1 份 0 号配料：成本 1 x 4 = 4
- 选择 2 份 1 号配料：成本 2 x 5 = 10
- 选择 0 份 2 号配料：成本 0 x 100 = 0
总成本：3 + 4 + 10 + 0 = 17 。不存在总成本为 18 的甜点制作方案。
```

**示例 3：**

```
输入：baseCosts = [3,10], toppingCosts = [2,5], target = 9
输出：8
解释：可以制作总成本为 8 和 10 的甜点。返回 8 ，因为这是成本更低的方案。
```

**示例 4：**

```
输入：baseCosts = [10], toppingCosts = [1], target = 1
输出：10
解释：注意，你可以选择不添加任何配料，但你必须选择一种基料。
```

**提示：**

- `n == baseCosts.length`
- `m == toppingCosts.length`
- `1 <= n, m <= 10`
- `1 <= baseCosts[i], toppingCosts[i] <= 10^4`
- `1 <= target <= 10^4`

**解法一：回溯**

**递归参数：** `index` ，表示当前需要考虑加入配料 `toppingCosts[index]` 

**终止条件：**

1. `index==len(toppingCosts)`  
2. 剪枝：当前花费 `cost ` 和 `target` 的距离更大时，直接剪枝，因为 `cost` 只会越来越大

**单层逻辑：** 

1. 要考虑更新 `ans` ，根据 `ans` 和 `target` 的距离和 `cost` 和 `target` 的距离决定，要注意，当前 `index` 层并没有考虑到 `toppingCosts[index]` 的价格（在下一层中考虑）所以这个处理逻辑要在 `index==len(toppingCosts)` 之前。
2. 要进入搜索树的下一层，考虑下一层配料的三种添加情况

```python
# 回溯法
# 对于每种基料，我都遍历所有的配料，每种配料可以加0个，1个，2个，然后记录花费中最接近 target 的
# 可以剪枝，对于大于 target 的花费 cost，如果 cost - target > abs(ans - target) 就直接返回了
# 因为 cost 是越来越大的
class Solution:
    def __init__(self):
        self.ans = 0
        self.cur_cost = 0

    def closestCost(self, baseCosts: List[int], toppingCosts: List[int], target: int) -> int:
        self.ans = min(baseCosts)
        for base_cost in baseCosts:
            self.cur_cost = base_cost
            self.backtracking(0, toppingCosts, target)
        return self.ans
    
    def backtracking(self, index, toppingCosts, target):
        # 剪枝
        if abs(self.ans - target) < self.cur_cost - target:
            return
        # 考虑当前花费是否可以更新答案
        # 如果距 target 更近就更新，如果一样近，就选小的
        if abs(self.ans - target) > abs(self.cur_cost - target):
            self.ans = self.cur_cost
        elif abs(self.ans - target) == abs(self.cur_cost - target):
            self.ans = min(self.ans, self.cur_cost)

        # 此结束条件一定要放在上一步之后，不能放在最开始，否则对于加了 toppingCosts[m-1] 的情况都没有判断
        if index == len(toppingCosts):
            return
        # 考虑 toppingCosts[index]
        for i in range(3):
            self.cur_cost += toppingCosts[index] * i
            self.backtracking(index + 1, toppingCosts, target)
            self.cur_cost -= toppingCosts[index] * i
```

