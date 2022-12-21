### [792. 匹配子序列的单词数](https://leetcode.cn/problems/number-of-matching-subsequences/)

- 中等

给定字符串 `s` 和字符串数组 `words`, 返回 *`words[i]` 中是`s`的子序列的单词个数* 。

字符串的 **子序列** 是从原始字符串中生成的新字符串，可以从中删去一些字符(可以是none)，而不改变其余字符的相对顺序。

- 例如， `“ace”` 是 `“abcde”` 的子序列。

**示例 1:**

```
输入: s = "abcde", words = ["a","bb","acd","ace"]
输出: 3
解释: 有三个是 s 的子序列的单词: "a", "acd", "ace"。
```

**示例 2:**

```
输入: s = "dsahjpjauf", words = ["ahjpjau","ja","ahbwzgqnuk","tnmlanowax"]
输出: 2
```

**提示:**

- `1 <= s.length <= 5 * 10^4`
- `1 <= words.length <= 5000`
- `1 <= words[i].length <= 50`
- `words[i]`和 s 都只由小写字母组成。

**解法一：哈希表+二分查找**

用哈希表记录 `s` 中每个字符的索引列表，然后遍历 `words` ，对每个 `word` 中的每个字符，查找当前字符的匹配索引

```python
class Solution:
    def numMatchingSubseq(self, s: str, words: List[str]) -> int:
        # 每个字符的索引位置列表
        pos = defaultdict(list)
        for i, c in enumerate(s):
            pos[c].append(i)
        
        ans = len(words)
        
        for w in words:
            # 长度更大，肯定不是子序列
            if len(w) > len(s):
                ans -= 1
                continue
            
            # p 是当前匹配字符的索引
            p = -1
            for c in w:
                # 取当前字符的索引列表，然后求比当前索引 p 第一个更大的索引位置
                ps = pos[c]
                j = bisect.bisect(ps, p)
                # 如果没找到
                if j == len(ps):
                    ans -= 1
                    break
                p = ps[j]
        return ans
```

