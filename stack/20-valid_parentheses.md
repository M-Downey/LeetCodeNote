### [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

- 简单

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。

**示例 1：**

```
输入：s = "()"
输出：true
```

**示例 2：**

```
输入：s = "()[]{}"
输出：true
```

**示例 3：**

```
输入：s = "(]"
输出：false
```

**示例 4：**

```
输入：s = "([)]"
输出：false
```

**示例 5：**

```
输入：s = "{[]}"
输出：true
```

**提示：**

- `1 <= s.length <= 104`
- `s` 仅由括号 `'()[]{}'` 组成

**解法一：栈**

遇到左括号入栈，遇到右括号出栈且要匹配

特殊情况：

1. 出栈，如果栈空，说明右括号多，直接返回`False`
2. 出栈，如果括号不匹配，返回`False`
3. 遍历结束，如果栈内还有，说明左括号多，返回`False`

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        pairs = {'(': ')', '{': '}', '[': ']'}
        for char in s:
            # 如果左括号，入栈
            if char in pairs:
                stack.append(char)
            # 如果遇到右括号，出栈
            else:
                if not stack:
                    return False
                # 如果不是对应的右括号
                elif pairs[stack[-1]] != char:
                    return False
                else:
                    stack.pop()
        if not stack:
            return True
        else:
            return False
```

