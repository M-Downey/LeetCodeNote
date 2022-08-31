### [946. 验证栈序列](https://leetcode.cn/problems/validate-stack-sequences/)

- 中等

给定 `pushed` 和 `popped` 两个序列，每个序列中的 **值都不重复**，只有当它们可能是在最初空栈上进行的推入 `push` 和弹出 `pop` 操作序列的结果时，返回 `true`；否则，返回 `false` 。

**示例 1：**

```
输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
输出：true
解释：我们可以按以下顺序执行：
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
```

**示例 2：**

```
输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
输出：false
解释：1 不能在 2 之前弹出。
```

**提示：**

- `1 <= pushed.length <= 1000`
- `0 <= pushed[i] <= 1000`
- `pushed` 的所有元素 **互不相同**
- `popped.length == pushed.length`
- `popped` 是 `pushed` 的一个排列

**解法一：遍历 `pushed`**

遍历 `pushed` ，把每个元素都入栈，入栈一个元素就遍历 `poped` ，如果栈顶元素等于 `poped[j]` ，那么就弹出

```python
class Solution:
    def validateStackSequences(self, pushed: List[int], popped: List[int]) -> bool:
        # 模拟入栈，判断是否可以出栈
        n = len(pushed)
        stack = []
        j = 0
        for i in range(n):
            stack.append(pushed[i])
            while stack and stack[-1] == popped[j]:
                stack.pop()
                j += 1
        return j == n
```

**解法二：遍历 `poped`**

遍历 `poped` ，每遍历一个元素 `poped[j]` ，如果和栈顶元素相等，就弹出；否则，需要遍历 `pushed` 把 `poped[j]` 之前的元素都入栈（找到一个相等的 `pushed[i]` ）；再否则，就返回 `False` 

```python
class Solution:
    def validateStackSequences(self, pushed: List[int], popped: List[int]) -> bool:
        # 要么在栈顶，要么在剩余的元素里
        stack = []
        i = 0
        n = len(pushed)
        for j in range(n):
            if stack and popped[j] == stack[-1]:
                stack.pop()
            elif popped[j] in pushed[i:]:
                while pushed[i] != popped[j]:
                    stack.append(pushed[i]) 
                    i += 1
                i += 1
            else:
                return False
        return True
```