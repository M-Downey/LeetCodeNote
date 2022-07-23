### [28. 实现 strStr()](https://leetcode.cn/problems/implement-strstr/)

- 简单

**重点：本题是KMP算法**

实现 [strStr()](https://baike.baidu.com/item/strstr/811469) 函数。

给你两个字符串 `haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 `needle` 字符串出现的第一个位置（下标从 0 开始）。如果不存在，则返回 `-1` 。

**说明：**

当 `needle` 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 `needle` 是空字符串时我们应当返回 0 。这与 C 语言的 [strstr()](https://baike.baidu.com/item/strstr/811469) 以及 Java 的 [indexOf()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)) 定义相符。

**示例 1：**

```
输入：haystack = "hello", needle = "ll"
输出：2
```

**示例 2：**

```
输入：haystack = "aaaaa", needle = "bba"
输出：-1
```

**提示：**

- `1 <= haystack.length, needle.length <= 104`
- `haystack` 和 `needle` 仅由小写英文字符组成

[**KMP算法：**](https://programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html)

用于字符串匹配，主要思想是：当模式串第j位失配时，j要指向模式串前面某个位置（满足新位置前面和主串前仍是匹配的），而不是从头匹配。

**构造next数组**

next数组中的第j位next[j]表示，当第j+1位与主串失配时，要将j移至next[j]的位置

**解法一：**

构造Next数组，然后i遍历主串，j遍历模式串

`i`是后缀结尾，`j`是前缀结尾（`needle[j]`已匹配，`haystack[i]`未匹配）

如果匹配成功，则在每次循环结尾，`j`应指向模式串最后一位，此时直接返回

跳出循环，说明没有匹配成功，返回`-1`

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        def getNext(s):
            Next = [0] * len(s)
            j = -1
            Next[0] = j
            # aabaa
            # j是前缀串的结尾（s[j]是匹配的）, i是后缀的结尾s[i]还未匹配
            for i in range(1, len(s)):
                # 如果失配，j要一直往前
                while j >= 0 and s[i] != s[j + 1]:
                    j = Next[j]
                # 如果匹配了，j是匹配前缀串的结尾，要加1
                if s[i] == s[j+1]:
                    j += 1
                # next[i]的值是匹配前后缀的长度-1，也就是j的值
                Next[i] = j
            return Next
        
        Next = getNext(needle)
        print(Next)
        j = -1
        for i in range(len(haystack)):
            print(i, j)
            while j >= 0 and haystack[i] != needle[j+1]:
                j =  Next[j]
            if haystack[i] == needle[j+1]:
                j += 1
            if j == len(needle) - 1:
                return i - j
        return -1
```

