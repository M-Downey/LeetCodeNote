### [187. 重复的DNA序列](https://leetcode.cn/problems/repeated-dna-sequences/)

- 中等

**DNA序列** 由一系列核苷酸组成，缩写为 `'A'`, `'C'`, `'G'` 和 `'T'`.。

- 例如，`"ACGAATTCCG"` 是一个 **DNA序列** 。

在研究 **DNA** 时，识别 DNA 中的重复序列非常有用。

给定一个表示 **DNA序列** 的字符串 `s` ，返回所有在 DNA 分子中出现不止一次的 **长度为 `10`** 的序列(子字符串)。你可以按 **任意顺序** 返回答案。

**题目说人话：就是找到重复了的长度为10的子字符串**

**示例 1：**

```
输入：s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"
输出：["AAAAACCCCC","CCCCCAAAAA"]
```

**示例 2：**

```
输入：s = "AAAAAAAAAAAAA"
输出：["AAAAAAAAAA"]
```

**提示：**

- `0 <= s.length <= `$10^5$
- `s[i]``==``'A'`、`'C'`、`'G'` or `'T'`

**解法一：哈希表**

```python
class Solution:
    def findRepeatedDnaSequences(self, s: str) -> List[str]:
        n = len(s)
        ht = defaultdict(int)
        ans = []
        # 最后一个字符串后面是 9 个字符，没有访问到
        for i in range(n - 9):
            ht[s[i: i + 10]] += 1
            if ht[s[i: i + 10]] == 2:
                ans.append(s[i: i + 10])
        return ans
```

**解法二：哈希表+滑动窗口+位运算**

每个字符串长度为10，且最多只有4种字符，可以将每种字符映射成2bit的数，这样每个字符串可以映射成一个唯一的20bit的数，然后用这个整数去做哈希

用长为10的窗口去遍历字符串，每次得到一个数x，用x做哈希，记录答案

每次得到数x，需要先左移2位，再与新数或运算，再取低20位

**注意，最后一次更新x后，没有计数和更新答案，要在循环外再判断一次**

```python
class Solution:
    def findRepeatedDnaSequences(self, s: str) -> List[str]:
        n = len(s)
        binary = {'A': 0, 'C': 1, 'G': 2, 'T': 3}
        x = 0
        # 先把前10个字符映射到 x 上
        for char in s[: 10]:
            x = (x << 2) | binary[char]
        ans = []
        ht = defaultdict(int)
        # 每次右移窗口
        for i in range(10, n):
            ht[x] += 1
            if ht[x] == 2:
                ans.append(s[i - 10: i])
            # ! 注意位运算的优先级低，要加括号
            x = ((x << 2) | binary[s[i]]) & ((1 << 20) - 1)
        # 最后一次修改 x ，并没有更新和检查
        ht[x] += 1
        if ht[x] == 2:
            ans.append(s[-10:])
        return ans
```

