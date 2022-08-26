### [132. 分割回文串 II](https://leetcode.cn/problems/palindrome-partitioning-ii/)

- 困难

给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是回文。

返回符合要求的 **最少分割次数** 。

**示例 1：**

```
输入：s = "aab"
输出：1
解释：只需一次分割就可将 s 分割成 ["aa","b"] 这样两个回文子串。
```

**示例 2：**

```
输入：s = "a"
输出：0
```

**示例 3：**

```
输入：s = "ab"
输出：1
```

**提示：**

- `1 <= s.length <= 2000`
- `s` 仅由小写英文字母组成

**解法一：动规**

```python
class Solution:
    def minCut(self, s: str) -> int:
        n = len(s)
        isPalindromic = [[False for _ in range(n)] for _ in range(n)]
        for i in range(n - 1, - 1, -1):
            for j in range(i, n):
                if s[i] == s[j] and ((j - i) <= 1 or isPalindromic[i + 1][j - 1] == True):
                    isPalindromic[i][j] = True
        
        # dp[i] 代表 s[0: i + 1] 中需要切割的最少分割数
        # dp[i] = min(dp[i], dp[j] + 1) for j in range(0, i) if isPalindromic[j + 1][i]
        dp = [i + 1 for i in range(n)]
        for i in range(n):
            # 如果 s[0: i + 1] 是回文串，它的分割数是0
            if isPalindromic[0][i]:
                dp[i] = 0
                continue
            for j in range(i):
                if isPalindromic[j + 1][i]:
                    dp[i] = min(dp[i], dp[j] + 1)
        return dp[n - 1]
```

