### [202. 快乐数](https://leetcode.cn/problems/happy-number/)

- 简单

编写一个算法来判断一个数 `n` 是不是快乐数。

**「快乐数」** 定义为：

- 对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
- 然后重复这个过程直到这个数变为 1，也可能是 **无限循环** 但始终变不到 1。

- 如果这个过程 **结果为** 1，那么这个数就是快乐数。

如果 `n` 是 *快乐数* 就返回 `true` ；不是，则返回 `false` 。

**示例 1：**

```
输入：n = 19
输出：true
解释：
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

**示例 2：**

```
输入：n = 2
输出：false
```

**提示：**

- `1 <= n <= 231 - 1`

**解法一：** 

快速判断：迭代的sum是否出现在一个集合中，用哈希

这里用record保存sum迭代的中间值

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        def sum(n):
            sum = 0
            while n:
                sum += (n % 10) ** 2
                n //= 10
            return sum

        record = []
        while True:
            # print(n)
            tmp = sum(n)
            # 如果出现循环
            if tmp in record:   # 底层实现应该是hash
                return False
            # 如果等于1
            elif tmp == 1:
                return True
            else:
                record.append(tmp)
            n = tmp
```

