### [1759. 统计同构子字符串的数目](https://leetcode.cn/problems/count-number-of-homogenous-substrings/)

- 中等

给你一个字符串 `s` ，返回 `s` 中 **同构子字符串** 的数目。由于答案可能很大，只需返回对 `109 + 7` **取余** 后的结果。

**同构字符串** 的定义为：如果一个字符串中的所有字符都相同，那么该字符串就是同构字符串。

**子字符串** 是字符串中的一个连续字符序列。

**示例 1：**

```
输入：s = "abbcccaa"
输出：13
解释：同构子字符串如下所列：
"a"   出现 3 次。
"aa"  出现 1 次。
"b"   出现 2 次。
"bb"  出现 1 次。
"c"   出现 3 次。
"cc"  出现 2 次。
"ccc" 出现 1 次。
3 + 1 + 2 + 1 + 3 + 2 + 1 = 13
```

**示例 2：**

```
输入：s = "xy"
输出：2
解释：同构子字符串是 "x" 和 "y" 。
```

**示例 3：**

```
输入：s = "zzzzz"
输出：15
```

**提示：**

- `1 <= s.length <= 10^5`
- `s` 由小写字符串组成

**解法一：数学**

观察可知，一段长为 `n` 的字符相同的子字符串所包含的同构字符串的个数为 
$$
\sum_{i=1}^ni=\frac{n*(n+1)}2
$$
所以遍历统计字符相同的子字符串长度，结果累加即可

```python
class Solution:
    def countHomogenous(self, s: str) -> int:
        # 每个长度为 n 的连续子字符串中的同构子字符串的数目是 (n + 1)*n / 2
        # 遍历统计字符相同的子字符串长度，结果累加即可
        n = len(s)
        pre = 0
        ans = 0
        MOD = 10 ** 9 + 7
        for i in range(1, n):
            if s[i] != s[pre]:
                sub_len = i - pre
                pre = i
                ans += ((sub_len + 1) * sub_len // 2) % MOD
        sub_len = n - pre
        ans += ((sub_len + 1) * sub_len // 2) % MOD
        return ans
```

