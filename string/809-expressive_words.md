### [809. 情感丰富的文字](https://leetcode.cn/problems/expressive-words/)

- 中等

有时候人们会用重复写一些字母来表示额外的感受，比如 `"hello" -> "heeellooo"`, `"hi" -> "hiii"`。我们将相邻字母都相同的一串字符定义为相同字母组，例如："h", "eee", "ll", "ooo"。

对于一个给定的字符串 S ，如果另一个单词能够通过将一些字母组扩张从而使其和 S 相同，我们将这个单词定义为可扩张的（stretchy）。扩张操作定义如下：选择一个字母组（包含字母 `c` ），然后往其中添加相同的字母 `c` 使其长度达到 3 或以上。

例如，以 "hello" 为例，我们可以对字母组 "o" 扩张得到 "hellooo"，但是无法以同样的方法得到 "helloo" 因为字母组 "oo" 长度小于 3。此外，我们可以进行另一种扩张 "ll" -> "lllll" 以获得 "helllllooo"。如果 `s = "helllllooo"`，那么查询词 "hello" 是可扩张的，因为可以对它执行这两种扩张操作使得 `query = "hello" -> "hellooo" -> "helllllooo" = s`。

输入一组查询单词，输出其中可扩张的单词数量。

**示例：**

```
输入： 
s = "heeellooo"
words = ["hello", "hi", "helo"]
输出：1
解释：
我们能通过扩张 "hello" 的 "e" 和 "o" 来得到 "heeellooo"。
我们不能通过扩张 "helo" 来得到 "heeellooo" 因为 "ll" 的长度小于 3 。
```

**提示：**

- `1 <= s.length, words.length <= 100`
- `1 <= words[i].length <= 100`
- s 和所有在 `words` 中的单词都只由小写字母组成。

**解法一：双指针**

```python
class Solution:
    def expressiveWords(self, s: str, words: List[str]) -> int:
        # 尝试扩充每个词 word 到 s
        # 双指针遍历 s 和 word
        # 记录 s 和 word 中同一个字符的数目，看 s 中的是否大于等于 word中的且>=3
        # 应该看s中每个字符数是否小于word中，或者小于3，切换状态
        ans = 0
        n = len(s)
        for word in words:
            m = len(word)
            i, j = 0, 0
            flag = 0
            while i < n and j < m:
                if s[i] != word[j]:
                    flag = 0
                    break
                c = s[i]
                cnti, cntj = 0, 0
                while i < n and c == s[i]:
                    i += 1
                    cnti += 1
                while j < m and c == word[j]:
                    j += 1
                    cntj += 1
                # print(c)
                # print(cnti, cntj)
                if cnti < cntj:
                    flag = 0
                    break
                if cnti != cntj and cnti < 3:
                    flag = 0
                    break
                flag = 1
            # 只有2个字符串都遍历完了才行
            if i == n and j == m:
                # print(word)
                ans += flag
        return ans
```

