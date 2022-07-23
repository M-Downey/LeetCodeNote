### [151. 颠倒字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

- 中等

给你一个字符串 `s` ，颠倒字符串中 **单词** 的顺序。

**单词** 是由非空格字符组成的字符串。`s` 中使用至少一个空格将字符串中的 **单词** 分隔开。

返回 **单词** 顺序颠倒且 **单词** 之间用单个空格连接的结果字符串。

**注意：**输入字符串 `s`中可能会存在前导空格、尾随空格或者单词间的多个空格。返回的结果字符串中，单词间应当仅用单个空格分隔，且不包含任何额外的空格。

**示例 1：**

```
输入：s = "the sky is blue"
输出："blue is sky the"
```

**示例 2：**

```
输入：s = "  hello world  "
输出："world hello"
解释：颠倒后的字符串中不能存在前导空格和尾随空格。
```

**示例 3：**

```
输入：s = "a good   example"
输出："example good a"
解释：如果两个单词间有多余的空格，颠倒后的字符串需要将单词间的空格减少到仅有一个。
```

**提示：**

- `1 <= s.length <= 104`
- `s` 包含英文大小写字母、数字和空格 `' '`
- `s` 中 **至少存在一个** 单词

**解法一：**

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        s = list(s)
        slow, fast = 0, 0
        # 去除前面空格
        while s[fast] == ' ' and fast < len(s):
            fast += 1
        
        # 双指针遍历，移动赋值
        for i in range(fast, len(s)):
            if i-1>0 and s[i] == s[i-1] and s[i] == ' ':
                continue
            else:
                s[slow] = s[i]
                slow += 1
        
        # 去除尾部空格，由于
        if slow-1 > 0 and s[slow-1] == ' ':
            s = s[:slow-1]
        else:
            s = s[:slow]
        
        def reverse(s, start, end):
            left ,right = start, end
            while left < right:
                s[left], s[right] = s[right], s[left]
                left += 1
                right -= 1
            return s[start: end+1]

        s = reverse(s, 0, len(s)-1)
        tmp = 0
        print(s)
        for i in range(len(s)):
            if s[i] == ' ':
                print(f'tmp={tmp}, i={i}')
                s[tmp: i] = reverse(s, tmp, i-1)
                print(s[tmp: i])
                tmp =  i+1
        s[tmp:] = reverse(s, tmp, len(s)-1)
        s = ''.join(s)
        print(s)
        return s
```

