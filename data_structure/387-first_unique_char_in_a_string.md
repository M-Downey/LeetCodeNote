### [387. 字符串中的第一个唯一字符](https://leetcode.cn/problems/first-unique-character-in-a-string/)

给定一个字符串 `s` ，找到 *它的第一个不重复的字符，并返回它的索引* 。如果不存在，则返回 `-1` 。

**示例 1：**

```
输入: s = "leetcode"
输出: 0
```

**示例 2:**

```
输入: s = "loveleetcode"
输出: 2
```

**示例 3:**

```
输入: s = "aabb"
输出: -1
```

**提示:**

- `1 <= s.length <= 105`
- `s` 只包含小写字母

**解法一：**哈希表，遍历字符串存储字符频率，再次遍历，找到频率为1的词语并返回索引

```python
# 我的代码
class Solution:
    def firstUniqChar(self, s: str) -> int:
        # hashtable
        char_frequency = [0] * 26
        for char in s:
            char_frequency[ord(char) - ord('a')] += 1
        for idx, char in enumerate(s):
            if char_frequency[ord(char) - ord('a')] == 1:
                return idx
        return -1
   
# 官方代码，用了Counter类(dict的子类)
class Solution:
    def firstUniqChar(self, s: str) -> int:
        frequency = collections.Counter(s) # dict
        for i, ch in enumerate(s):
            if frequency[ch] == 1:
                return i
        return -1
```

**方法二： 使用哈希表存储索引**

**思路与算法**

我们可以对方法一进行修改，使得第二次遍历的对象从字符串变为哈希映射。

第一次遍历，构建哈希映射，第一次出现，`value`存为索引，第二次出现，`value`改为`-1`

在第一次遍历结束后，我们只需要再遍历一次哈希映射中的所有值，找出其中不为 `-1 `的最小值，即为第一个不重复字符的索引。如果哈希映射中的所有值均为 `-1`（不存在唯一的字符），我们就返回 `-1`。

```python
class Solution:
    def firstUniqChar(self, s: str) -> int:
        position = dict()
        n = len(s)
        for i, ch in enumerate(s):
            if ch in position:
                position[ch] = -1
            else:
                position[ch] = i
        first = n
        for pos in position.values():
            if pos != -1 and pos < first:
                first = pos
        if first == n:
            first = -1
        return first
```

