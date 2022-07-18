### [383. 赎金信](https://leetcode.cn/problems/ransom-note/)

- 简单

给你两个字符串：`ransomNote` 和 `magazine` ，判断 `ransomNote` 能不能由 `magazine` 里面的字符构成。

如果可以，返回 `true` ；否则返回 `false` 。

`magazine` 中的每个字符只能在 `ransomNote` 中使用一次。

 **示例 1：**

```
输入：ransomNote = "a", magazine = "b"
输出：false
```

**示例 2：**

```
输入：ransomNote = "aa", magazine = "ab"
输出：false
```

**示例 3：**

```
输入：ransomNote = "aa", magazine = "aab"
输出：true
```

**提示：**

- `1 <= ransomNote.length, magazine.length <= 105`
- `ransomNote` 和 `magazine` 由小写英文字母组成

**解法一：**单哈希表

遍历得到赎金信的`word: count`，再遍历`magazine`，`count`自减，最后检查，如果`count`大于`0`，说明`magzine`中的词不够用

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        # 单哈希表方法
        ht = {}
        for char in ransomNote:
            ht[char] = ht.get(char, 0) + 1
        for char in magazine:
            ht[char] = ht.get(char, 0) - 1
            
        alphabet = 'abcdefghijklmnopqrstuvwxyz'
        for char in alphabet:
            # magazine不够用
            if ht.get(char, 0) > 0:
                return False
        return True
```

官方题解中调`Counter`的做法：

```python
# 先看Counter类中的加减法
# 没有的k: v，默认v = 0
# 删除结果v <= 0的项
>>> x
{'a': 1, 'b': 2}
>>> y
{'b': 3, 'd': -4}
>>> Counter(x)+Counter(y)
Counter({'b': 5, 'a': 1})
>>> Counter(x)-Counter(y)
Counter({'d': 4, 'a': 1})
```

```python
# ransomNote应该“小于等于”magazine，相减后是NULL
# 故用not collections.Counter(ransomNote) - collections.Counter(magazine)
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        if len(ransomNote) > len(magazine):
            return False
        return not collections.Counter(ransomNote) - collections.Counter(magazine)
```

