### [459. 重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/)

- 简单

给定一个非空的字符串 `s` ，检查是否可以通过由它的一个子串重复多次构成。

**示例 1:**

```
输入: s = "abab"
输出: true
解释: 可由子串 "ab" 重复两次构成。
```

**示例 2:**

```
输入: s = "aba"
输出: false
```

**示例 3:**

```
输入: s = "abcabcabcabc"
输出: true
解释: 可由子串 "abc" 重复四次构成。 (或子串 "abcabc" 重复两次构成。)
```

**提示：**

- `1 <= s.length <= 104`
- `s` 由小写英文字母组成

**解法一：KMP算法**

求next数组即可，注意观察next数组的规律，一定是[-1, -1, -1, 0, 1, 2, 3, 4, 5]这种

 ![459.重复的子字符串_1](https://code-thinking.cdn.bcebos.com/pics/459.%E9%87%8D%E5%A4%8D%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2_1.png)

```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        def getNext(s):
            Next = [0] * len(s)
            j = -1
            Next[0] = j
            for i in range(1, len(s)):
                while j >= 0 and s[i] != s[j+1]:
                    j = Next[j]
                if s[i] == s[j+1]:
                    j += 1
                Next[i] = j
            return Next
        
        Next = getNext(s)
        # print(Next)
        n = len(s)
        if Next[n-1] != -1 and (n % (n - (Next[n-1] + 1)) == 0):
            return True
        return False
```

