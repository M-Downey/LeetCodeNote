### [49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)

- 中等

给你一个字符串数组，请你将 **字母异位词** 组合在一起。可以按任意顺序返回结果列表。

**字母异位词** 是由重新排列源单词的字母得到的一个新单词，所有源单词中的字母通常恰好只用一次。

**示例 1:**

```
输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
输出: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

**示例 2:**

```
输入: strs = [""]
输出: [[""]]
```

**示例 3:**

```
输入: strs = ["a"]
输出: [["a"]]
```

**提示：**

- `1 <= strs.length <= 10^4`
- `0 <= strs[i].length <= 100`
- `strs[i]` 仅包含小写字母

**解法一：哈希表**

排序后相同的字符串放在一起

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        ht = collections.defaultdict(list)
        for s in strs:
            # sorted返回的是列表
            key = ''.join(sorted(s))
            ht[key].append(s)
        return list(ht.values())
```

