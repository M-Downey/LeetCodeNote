### [5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)

- 中等

给你一个字符串 `s`，找到 `s` 中最长的回文子串。

**示例 1：**

```
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
```

**示例 2：**

```
输入：s = "cbbd"
输出："bb"
```

**提示：**

- `1 <= s.length <= 1000`
- `s` 仅由数字和英文字母组成

**解法一：动规**

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        # dp[i][j] 代表 s[i: j+1] 是否是回文子串
        # dp[i][j] = (s[i]==s[j] && (j-i) <= 1) || (s[i]==s[j] && dp[i+1][j-1])
        n = len(s)
        dp = [[False for _ in range(n)] for _ in range(n)]
        maxLen = 0
        left, right = 0, 0
        for i in range(n - 1, -1, -1):
            for j in range(i, n):
                if s[i] == s[j]:
                    if j - i <= 1:
                        dp[i][j] = True
                    elif dp[i + 1][j - 1]:
                        dp[i][j] = True
                if dp[i][j] and (j - i + 1) > maxLen:
                    maxLen = j - i + 1
                    left, right = i, j
        return s[left: right + 1]
```

