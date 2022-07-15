### [1002. 查找共用字符](https://leetcode.cn/problems/find-common-characters/)

- 简单

给你一个字符串数组 `words` ，请你找出所有在 `words` 的每个字符串中都出现的共用字符（ **包括重复字符**），并以数组形式返回。你可以按 **任意顺序** 返回答案。

**示例 1：**

```
输入：words = ["bella","label","roller"]
输出：["e","l","l"]
```

**示例 2：**

```
输入：words = ["cool","lock","cook"]
输出：["c","o"]
```

**提示：**

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 100`
- `words[i]` 由小写英文字母组成

**解法一：**

**关键：**小写字母，频率 -> 就是用哈希表

整体思路就是统计出搜索字符串里26个字符的出现的频率，然后取每个字符频率最小值，最后转成输出格式就可以了。

![1002.查找常用字符](https://camo.githubusercontent.com/d7f165b43baffeaed8114c1a8396081b46f9b2ad8a264217e8d2b99f5cf5af49/68747470733a2f2f636f64652d7468696e6b696e672e63646e2e626365626f732e636f6d2f706963732f313030322e2545362539462541352545362538392542452545352542382542382545372539342541382545352541442539372545372541432541362e706e67)

空间优化：用一个哈希表记录所有词的历史最小频率，用另一个哈希表记录当前词的频率，两者

```python
# python dict实现
class Solution:
    def commonChars(self, words: List[str]) -> List[str]:
        hashtable = {}
        for char in words[0]:
            hashtable[char] = hashtable.get(char, 0) + 1
        
        alphabet = 'abcdefghijklmnopqrstuvwxyz'
        for word in words[1:]:
            hashtable2 = {}
            for char in word:
                hashtable2[char] = hashtable2.get(char, 0) + 1
            for c in alphabet:
                hashtable[c] = min(hashtable.get(c, 0), hashtable2.get(c, 0))
        res = []
        for k, v in hashtable.items():
            for i in range(v):
                res.append(k)

        return res
```

