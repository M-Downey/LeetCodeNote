### [2055. 蜡烛之间的盘子](https://leetcode.cn/problems/plates-between-candles/)

- 中等

给你一个长桌子，桌子上盘子和蜡烛排成一列。给你一个下标从 **0** 开始的字符串 `s` ，它只包含字符 `'*'` 和 `'|'` ，其中 `'*'` 表示一个 **盘子** ，`'|'` 表示一支 **蜡烛** 。

同时给你一个下标从 **0** 开始的二维整数数组 `queries` ，其中 `queries[i] = [lefti, righti]` 表示 **子字符串** `s[lefti...righti]` （**包含左右端点的字符**）。对于每个查询，你需要找到 **子字符串中** 在 **两支蜡烛之间** 的盘子的 **数目** 。如果一个盘子在 **子字符串中** 左边和右边 **都** 至少有一支蜡烛，那么这个盘子满足在 **两支蜡烛之间** 。

- 比方说，`s = "||**||**|*"` ，查询 `[3, 8]` ，表示的是子字符串 `"*||***\**\***|"` 。子字符串中在两支蜡烛之间的盘子数目为 `2` ，子字符串中右边两个盘子在它们左边和右边 **都** 至少有一支蜡烛。

请你返回一个整数数组 `answer` ，其中 `answer[i]` 是第 `i` 个查询的答案。

**示例 1:**

 ![ex-1](https://assets.leetcode.com/uploads/2021/10/04/ex-1.png)

```
输入：s = "**|**|***|", queries = [[2,5],[5,9]]
输出：[2,3]
解释：
- queries[0] 有两个盘子在蜡烛之间。
- queries[1] 有三个盘子在蜡烛之间。
```

**示例 2:**

 ![ex-2](https://assets.leetcode.com/uploads/2021/10/04/ex-2.png)

```
输入：s = "***|**|*****|**||**|*", queries = [[1,17],[4,5],[14,17],[5,11],[15,16]]
输出：[9,0,0,0,0]
解释：
- queries[0] 有 9 个盘子在蜡烛之间。
- 另一个查询没有盘子在蜡烛之间。
```

**提示：**

- `3 <= s.length <= 10^5`
- `s` 只包含字符 `'*'` 和 `'|'` 。
- `1 <= queries.length <= 10^5`
- `queries[i].length == 2`
- `0 <= lefti <= righti < s.length`

**解法一：二分插入+数学**

对每个 `query` 来说，只需要找到在 `query` 对应区间内，最左边和最右边的蜡烛位置即可，通过左右蜡烛之间的物品数目和蜡烛数目即可得到盘子数目

所以考虑把每个蜡烛的位置记录下来，构成数组 `nums`

对于每个 `query` 的区间 `[left, right]` 来说，要找到第一个大于等于 `left` 的蜡烛位置在 `nums` 中对应的索引 `idx1` 和第一个小于等于 `right` 的蜡烛位置在 `nums` 中对应的索引 `idx2`

然后就可以得到两支蜡烛的位置 `nums[idx1]` 和 `nums[idx2]` 以及蜡烛的数量 `idx2 - idx1 + 1`

那么就可以计算盘子的数量

在具体实现的时候，用 `bisect` 查找

```python
idx1 = bisect.bisect_left(nums, left)
idx2 = bisect.bisect_right(nums, right)
```

这里找到的 `idx1` 刚好是最左边的蜡烛的位置

`idx2` 是最右边蜡烛的后一位

需要注意的是

-  `query` 区间内可能没有蜡烛，那么找到的 `idx1 = idx2` 需要把这种情况跳过，直接计0
- 对于区间内只有 1 根蜡烛的情况，对应的 `idx1` 和 `idx2` 在 `nums` 中刚好相邻差1，计算结果也为0，所以不用特殊考虑了

```python
class Solution:
    def platesBetweenCandles(self, s: str, queries: List[List[int]]) -> List[int]:
        # 找到所有蜡烛的下标组成数组 nums[3, 6, 12, 15, 16, 19]
        # 对于每个 query 找到它左右边界, 第一个大于等于它query[0]的，第一个小于等于query[1]的
        # idx1 idx2
        # 盘子数是 nums[idx2] - nums[idx1] + 1 - (idx2 - idx1 + 1)
        n = len(s)
        nums = []
        for i in range(n):
            if s[i] == '|':
                nums.append(i)
        # print(nums)
        ans = []
        for left, right in queries:
            idx1 = bisect.bisect_left(nums, left)
            idx2 = bisect.bisect_right(nums, right)
            # print(idx1, idx2)
            if idx1 == idx2:
                ans.append(0)
                continue
            cnt = nums[idx2 - 1] - nums[idx1] + 1 - (idx2 - 1 - idx1 + 1)
            ans.append(cnt)
        return ans
```

