### [754. 到达终点数字](https://leetcode.cn/problems/reach-a-number/)

- 中等

在一根无限长的数轴上，你站在`0`的位置。终点在`target`的位置。

你可以做一些数量的移动 `numMoves` :

- 每次你可以选择向左或向右移动。
- 第 `i` 次移动（从  `i == 1` 开始，到 `i == numMoves` ），在选择的方向上走 `i` 步。

给定整数 `target` ，返回 *到达目标所需的 **最小** 移动次数(即最小 `numMoves` )* 。

 **示例 1:**

```
输入: target = 2
输出: 3
解释:
第一次移动，从 0 到 1 。
第二次移动，从 1 到 -1 。
第三次移动，从 -1 到 2 。
```

**示例 2:**

```
输入: target = 3
输出: 2
解释:
第一次移动，从 0 到 1 。
第二次移动，从 1 到 3 。
```

**提示:**

- `-10^9 <= target <= 10^9`
- `target != 0`

**解法一：数学**

1. 只需考虑正数的 `target`

2. 设 `k` 为满足 $s = \sum_{i=1}^{k} \geq \textit{target}$ 的最小正整数，如果 `s=target` 那么答案就是 `k`，如果 $s > target$ 需要在部分整数前添加负号，使 `s` 减小
3. 如果 $delta = s - target$ 为偶数，那么只需要在前 `k` 个数中找到若干个数的和等于 $\frac{delta}{2}$ ，并且一定存在若干个整数，其和为上述值
4. 如果 $delta$ 为奇数，就凑不出来，需要继续使用 $k+1$ 或 $k+2$ ，因为 $delta$ 为奇数，说明 $s$ 和 $target$ 奇偶性不同，我们需要改变 $s$ 的奇偶性，
   - 对于 k 和 s 的奇偶性，分4种情况
   - k奇，s奇
   - k偶，s奇
   - k奇，s偶
   - k偶，s偶
   - 可以发现，对于 k 是奇数的情况，需要 k 增大两位才能改变 s 的奇偶性，对于 k 是偶数，k 只需要增大一位就可以改变奇偶性

```python
class Solution:
    def reachNumber(self, target: int) -> int:
        target = abs(target)
        k = 0
        while target > 0:
            k += 1
            target -= k
        return k if target % 2 == 0 else k + 1 + k % 2
```

