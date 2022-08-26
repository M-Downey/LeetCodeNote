### [127. 单词接龙](https://leetcode.cn/problems/word-ladder/)

- 困难

字典 `wordList` 中从单词 `beginWord` 和 `endWord` 的 **转换序列** 是一个按下述规格形成的序列 `beginWord -> s1 -> s2 -> ... -> sk`：

- 每一对相邻的单词只差一个字母。

-  对于 `1 <= i <= k` 时，每个 `si` 都在 `wordList` 中。注意， `beginWord` 不需要在 `wordList` 中。

- `sk == endWord`

给你两个单词 `beginWord` 和 `endWord` 和一个字典 `wordList` ，返回 *从 `beginWord` 到 `endWord` 的 **最短转换序列** 中的 **单词数目*** 。如果不存在这样的转换序列，返回 `0` 。

**示例 1：**

```
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
输出：5
解释：一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog", 返回它的长度 5。
```

**示例 2：**

```
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
输出：0
解释：endWord "cog" 不在字典中，所以无法进行转换。
```

**提示：**

- `1 <= beginWord.length <= 10`
- `endWord.length == beginWord.length`

- `1 <= wordList.length <= 5000`
- `wordList[i].length == beginWord.length`

- `beginWord`、`endWord` 和 `wordList[i]` 由小写英文字母组成
- `beginWord != endWord`

- `wordList` 中的所有字符串 **互不相同**

**解法一：BFS**

`BFS` 每次从队头选出的单词，把它每个位置上的字母改变后，放入队列中，并记录其距离（注意已经记录过的基因，不能再记录），如果单词等于目标单词，就返回距离

```python
from collections import deque
class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        # 需要这个，否则超时
        wordSet = set(wordList)
        if endWord not in wordSet:
            return 0
        queue = deque([beginWord])
        # 记录已找到词语的距离
        wordDistance = {beginWord: 1}
        while queue:
            word = queue.popleft()
            # 每次替换一个位置上的一个字母
            distance = wordDistance[word]
            for i in range(len(word)):
                newWord = list(word)
                # print(newWord)
                for j in range(26):
                    newWord[i] = chr(97 + j)
                    newWord = "".join(newWord)
                    if newWord == endWord:
                        return distance + 1
                    if newWord in wordSet and newWord not in wordDistance:
                        wordDistance[newWord] = distance + 1
                        queue.append(newWord)
                    newWord = list(newWord) # 需要再转成列表才能改变值
        return 0
```

