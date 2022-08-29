### [690. 员工的重要性](https://leetcode.cn/problems/employee-importance/)

- 中等

给定一个保存员工信息的数据结构，它包含了员工 **唯一的 id** ，**重要度** 和 **直系下属的 id** 。

比如，员工 1 是员工 2 的领导，员工 2 是员工 3 的领导。他们相应的重要度为 15 , 10 , 5 。那么员工 1 的数据结构是 [1, 15, [2]] ，员工 2的 数据结构是 [2, 10, [3]] ，员工 3 的数据结构是 [3, 5, []] 。注意虽然员工 3 也是员工 1 的一个下属，但是由于 **并不是直系** 下属，因此没有体现在员工 1 的数据结构中。

现在输入一个公司的所有员工信息，以及单个员工 id ，返回这个员工和他所有下属的重要度之和。

**示例：**

```
输入：[[1, 5, [2, 3]], [2, 3, []], [3, 3, []]], 1
输出：11
解释：
员工 1 自身的重要度是 5 ，他有两个直系下属 2 和 3 ，而且 2 和 3 的重要度均为 3 。因此员工 1 的总重要度是 5 + 3 + 3 = 11 。
```

**提示：**

- 一个员工最多有一个 **直系** 领导，但是可以有多个 **直系** 下属
- 员工数量不超过 2000 。

**解法一：DFS**

```python
"""
# Definition for Employee.
class Employee:
    def __init__(self, id: int, importance: int, subordinates: List[int]):
        self.id = id
        self.importance = importance
        self.subordinates = subordinates
"""

class Solution:
    def __init__(self):
        self.ans = 0
        self.id2node = {}

    def getImportance(self, employees: List['Employee'], id: int) -> int:
        self.id2node = {employee.id: employee for employee in employees}
        self.dfs(id)
        return self.ans

    def dfs(self, id):
        node = self.id2node[id]
        self.ans += node.importance
        for subordinate in node.subordinates:
            self.dfs(subordinate)
```

