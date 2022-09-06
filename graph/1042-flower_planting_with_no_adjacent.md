### [1042. 不邻接植花](https://leetcode.cn/problems/flower-planting-with-no-adjacent/)

- 中等

有 `n` 个花园，按从 `1` 到 `n` 标记。另有数组 `paths` ，其中 `paths[i] = [xi, yi]` 描述了花园 `xi` 到花园 `yi` 的双向路径。在每个花园中，你打算种下四种花之一。

另外，所有花园 **最多** 有 **3** 条路径可以进入或离开.

你需要为每个花园选择一种花，使得通过路径相连的任何两个花园中的花的种类互不相同。

*以数组形式返回 **任一** 可行的方案作为答案 `answer`，其中 `answer[i]` 为在第 `(i+1)` 个花园中种植的花的种类。花的种类用  1、2、3、4 表示。保证存在答案。*

**示例 1：**

```
输入：n = 3, paths = [[1,2],[2,3],[3,1]]
输出：[1,2,3]
解释：
花园 1 和 2 花的种类不同。
花园 2 和 3 花的种类不同。
花园 3 和 1 花的种类不同。
因此，[1,2,3] 是一个满足题意的答案。其他满足题意的答案有 [1,2,4]、[1,4,2] 和 [3,2,1]
```

**示例 2：**

```
输入：n = 4, paths = [[1,2],[3,4]]
输出：[1,2,1,2]
```

**示例 3：**

```
输入：n = 4, paths = [[1,2],[2,3],[3,4],[4,1],[1,3],[2,4]]
输出：[1,2,3,4]
```

**提示：**

- `1 <= n <= 10^4`
- `0 <= paths.length <= 2 * 10^4`

- `paths[i].length == 2`
- `1 <= xi, yi <= n`

- `xi != yi`
- 每个花园 **最多** 有 **3** 条路径可以进入或离开

**解法一：建图模拟**

用邻接表构建图后，为每个花园种花，种花时要用总花减去它周围花园的花，然后选一个可用的花种即可

学习了集合的差操作

```python
class Solution:
    def gardenNoAdj(self, n: int, paths: List[List[int]]) -> List[int]:
        # 给每个花园周围的种上与周围花园不一样的花
        graph = defaultdict(list)
        for u, v in paths:
            graph[u].append(v)
            graph[v].append(u)
        ans = [0] * n
        all_flowers = set([1, 2, 3, 4])
        # 除去当前花园 i 的相邻花园的花
        for i in range(1, n + 1):
            left_flowers = all_flowers - set([ans[v - 1] for v in graph[i]])
            # 随机选一个可用的花
            ans[i - 1] = left_flowers.pop()
        return ans
```

