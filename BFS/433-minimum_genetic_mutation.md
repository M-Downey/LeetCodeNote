### [433. 最小基因变化](https://leetcode.cn/problems/minimum-genetic-mutation/)

- 中等

基因序列可以表示为一条由 8 个字符组成的字符串，其中每个字符都是 `'A'`、`'C'`、`'G'` 和 `'T'` 之一。

假设我们需要调查从基因序列 `start` 变为 `end` 所发生的基因变化。一次基因变化就意味着这个基因序列中的一个字符发生了变化。

- 例如，`"AACCGGTT" --> "AACCGGTA"` 就是一次基因变化。

另有一个基因库 `bank` 记录了所有有效的基因变化，只有基因库中的基因才是有效的基因序列。（变化后的基因必须位于基因库 `bank` 中）

给你两个基因序列 `start` 和 `end` ，以及一个基因库 `bank` ，请你找出并返回能够使 `start` 变化为 `end` 所需的最少变化次数。如果无法完成此基因变化，返回 `-1` 。

注意：起始基因序列 `start` 默认是有效的，但是它并不一定会出现在基因库中。

**示例 1：**

```
输入：start = "AACCGGTT", end = "AACCGGTA", bank = ["AACCGGTA"]
输出：1
```

**示例 2：**

```
输入：start = "AACCGGTT", end = "AAACGGTA", bank = ["AACCGGTA","AACCGCTA","AAACGGTA"]
输出：2
```

**示例 3：**

```
输入：start = "AAAAACCC", end = "AACCCCCC", bank = ["AAAACCCC","AAACCCCC","AACCCCCC"]
输出：3
```

**提示：**

- `start.length == 8`
- `end.length == 8`

- `0 <= bank.length <= 10`
- `bank[i].length == 8`

- `start`、`end` 和 `bank[i]` 仅由字符 `['A', 'C', 'G', 'T']` 组成

**解法一：BFS**

`BFS` 每次从队头选出的基因，把它每个位置上的基因改变后，放入队列中，并记录其距离（注意已经记录过的基因，不能再记录），如果基因等于目标基因，就返回距离

```python
class Solution:
    def minMutation(self, start: str, end: str, bank: List[str]) -> int:
        # bank最长10，应该可以不转换成set
        bankSet = set(bank)
        if end not in bankSet:
            return -1
        q = deque([start])
        distance = {start: 0}
        G = "ACGT"
        while q:
            gene = q.popleft()
            dist = distance[gene]
            # 每次替换一个位置上的基因
            for i in range(len(gene)):
                newGene = list(gene)
                for j in range(4):
                    newGene[i] = G[j]
                    newGene = ''.join(newGene)
                    if newGene == end:
                        return dist + 1
                    if newGene in bankSet and newGene not in distance:
                        distance[newGene] = dist + 1
                        q.append(newGene)
                    newGene = list(newGene)
        return -1
```

