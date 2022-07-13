### [242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

给定两个字符串 `s` 和` t` ，编写一个函数来判断` t` 是否是 `s `的字母异位词。

注意：若 `s `和 `t` 中每个字符出现的次数都相同，则称` s` 和 `t `互为字母异位词。

**示例 1:**

```
输入: s = "anagram", t = "nagaram"
输出: true
```

**示例 2:**

```
输入: s = "rat", t = "car"
输出: false
```

**提示:**

- `1 <= s.length, t.length <= 5 * 104`
- `s` 和 `t` 仅包含小写字母

**进阶:** 如果输入字符串包含 `unicode `字符怎么办？你能否调整你的解法来应对这种情况？

**解法一：**哈希表，求词频，然后遍历是否相等

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if set(s) != set(t):
            return False
        dic1, dic2 = {}, {}
        for c in s:
            dic1[c] = dic1.get(c, 0) + 1
        for c in t:
            dic2[c] = dic2.get(c, 0) + 1
        # print(dic1, dic2)
        for key in dic1.keys():
            if dic1[key] != dic2[key]:
                return False
        return True
```

