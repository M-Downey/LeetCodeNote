### [22. 括号生成](https://leetcode.cn/problems/generate-parentheses/)

- 中等

数字 `n` 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

**示例 1：**

```
输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]
```

**示例 2：**

```
输入：n = 1
输出：["()"]
```

**提示：**

- `1 <= n <= 8`

**解法一：DFS + 剪枝**

`DFS` 生成所有的括号，根据括号成对出现进行剪枝，如果左括号的个数小于右括号，就剪枝，或者是左括号的数量大于 `n`

```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        def dfs(path, left_num, right_num):
            if left_num < right_num:
                return
            if left_num > n or right_num > n:
                return
            if len(path) == 2 * n:
                ans.append(path)
                return
            dfs(path + '(', left_num + 1, right_num)
            dfs(path + ')', left_num, right_num + 1)
        ans = []
        dfs('', 0, 0)
        return ans
```

