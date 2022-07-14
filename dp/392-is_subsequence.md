### [392. 判断子序列](https://leetcode.cn/problems/is-subsequence/)

- 简单

给定字符串 **s** 和 **t** ，判断 **s** 是否为 **t** 的子序列。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，`"ace"`是`"abcde"`的一个子序列，而`"aec"`不是）。

**进阶：**

如果有大量输入的 S，称作 S1, S2, ... , Sk 其中 k >= 10亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？

**示例 1：**

```
输入：s = "abc", t = "ahbgdc"
输出：true
```

**示例 2：**

```
输入：s = "axc", t = "ahbgdc"
输出：false
```

**提示：**

- `0 <= s.length <= 100`
- `0 <= t.length <= 10^4`
- 两个字符串都只由小写字符组成。

**解法一：**双指针遍历

这样，我们初始化两个指针 `i` 和 `j`，分别指向 `s` 和 `t` 的初始位置。每次贪心地匹配，匹配成功则 `i` 和 `j` 同时右移，匹配 `s` 的下一个位置，匹配失败则 `j` 右移，`i` 不变，尝试用 `t` 的下一个字符匹配 `s`。

最终如果 `i` 移动到 `s` 的末尾，就说明 `s` 是 `t` 的子序列。

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        i, j = 0, 0
        s_len, t_len = len(s), len(t)
        while(i < s_len):
            if j >= t_len:
                break
            # if 相等，都+1
            if s[i] == t[j]:
                i += 1
                j += 1
            else:#不等，j指针后移，匹配下一个
                j += 1
        if i == s_len:
            return True
        else:
            return False
```

