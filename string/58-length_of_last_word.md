### [58. 最后一个单词的长度](https://leetcode.cn/problems/length-of-last-word/)

- 简单

给你一个字符串 `s`，由若干单词组成，单词前后用一些空格字符隔开。返回字符串中 **最后一个** 单词的长度。

**单词** 是指仅由字母组成、不包含任何空格字符的最大子字符串。

**示例 1：**

```
输入：s = "Hello World"
输出：5
解释：最后一个单词是“World”，长度为5。
```

**示例 2：**

```
输入：s = "   fly me   to   the moon  "
输出：4
解释：最后一个单词是“moon”，长度为4。
```

**示例 3：**

```
输入：s = "luffy is still joyboy"
输出：6
解释：最后一个单词是长度为6的“joyboy”。
```

**提示：**

- `1 <= s.length <= 10^4`
- `s` 仅有英文字母和空格 `' '` 组成
- `s` 中至少存在一个单词

**解法一：模拟**

找到最后一个单词的前一位索引和最后一个单词的最后一个字母的索引，相减即可

注意， `left` 初始值是 `-1`

```python
class Solution:
    def lengthOfLastWord(self, s: str) -> int:
        n = len(s)
        # left 是最后一个单词前的空格的索引，right 是最后一个单词的最后一个字母的索引
        left, right = -1, n - 1
        for i in range(n - 1, -1, -1):
            if s[i] != ' ':
                right = i
                break
        for i in range(right + 1):
            if s[i] == ' ':
                left = i
        return right - left
```

