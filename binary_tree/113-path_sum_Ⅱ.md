### [113. 路径总和 II](https://leetcode.cn/problems/path-sum-ii/)

- 中等

给你二叉树的根节点 `root` 和一个整数目标和 `targetSum` ，找出所有 **从根节点到叶子节点** 路径总和等于给定目标和的路径。

**叶子节点** 是指没有子节点的节点。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg)

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：[[5,4,11,2],[5,8,4,5]]
# 返回的是node.val，不是node
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg)

```
输入：root = [1,2,3], targetSum = 5
输出：[]
```

**示例 3：**

```
输入：root = [1,2], targetSum = 0
输出：[]
```

**提示：**

- 树中节点总数在范围 `[0, 5000]` 内
- `-1000 <= Node.val <= 1000`
- `-1000 <= targetSum <= 1000`

**解法一：BFS**

​	用`now_sum`，`next_sum`数组记录当前层`q`对应的路径和以及下一层`tmp`对应的路径和	

​	访问的话，为每层的节点加上索引属性`idx`，访问这层的路径和用`idx`，访问下层的用`node.left.val`或`node.right.val`，同时为下层节点`idx`赋值，用`len(tmp)`

​	每找到一个满足条件的叶子节点，就要找它的路径，用哈希表记录每个节点的父节点，然后用辅助函数`findPath`找出一个节点到根节点的路径并加到`ans`中

总结：我的BFS思路和“层“总是绑定在一起了，而**这道题的BFS不需要按一层一层的遍历，只需一个一个遍历即可**，反倒是按层遍历，需要考虑新的一层的每个节点的路径和更新问题，涉及到索引，就需要为每个节点新添加一个属性，就更麻烦。

如果一个个遍历，只需多一个`sum`队列，对应每个节点的父节点的路径和

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import defaultdict
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        if not root:
            return []
        ans = []
        parent = defaultdict(lambda: None)
        q = [(root, 0)]
        now_sum = [root.val]
        def findPath(node):
            path = []
            while node:
                path.append(node.val)
                node = parent[node]
            return path[::-1]
        while q:
            for i in range(len(now_sum)):
                if not q[i][0].left and not q[i][0].right and now_sum[i] == targetSum:
                    ans.append(findPath(q[i][0]))
            tmp = []
            next_sum = []
            for node, idx in q:
                if node.left:
                    tmp.append((node.left, len(tmp)))
                    next_sum.append(now_sum[idx] + node.left.val)
                    parent[node.left] = node
                if node.right:
                    tmp.append((node.right, len(tmp)))
                    next_sum.append(now_sum[idx] + node.right.val)
                    parent[node.right] = node
            q = tmp
            now_sum = next_sum
        return ans
```

官方题解：

```python
class Solution:
    def pathSum(self, root: TreeNode, targetSum: int) -> List[List[int]]:
        ret = list()
        parent = collections.defaultdict(lambda: None)

        def getPath(node: TreeNode):
            tmp = list()
            while node:
                tmp.append(node.val)
                node = parent[node]
            ret.append(tmp[::-1])

        if not root:
            return ret
        
        que_node = collections.deque([root])
        que_total = collections.deque([0])

        while que_node:
            node = que_node.popleft()
            rec = que_total.popleft() + node.val

            if not node.left and not node.right:
                if rec == targetSum:
                    getPath(node)
            else:
                if node.left:
                    parent[node.left] = node
                    que_node.append(node.left)
                    que_total.append(rec)
                if node.right:
                    parent[node.right] = node
                    que_node.append(node.right)
                    que_total.append(rec)

        return ret

作者：LeetCode-Solution
链接：https://leetcode.cn/problems/path-sum-ii/solution/lu-jing-zong-he-ii-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

