### [501. 二叉搜索树中的众数](https://leetcode.cn/problems/find-mode-in-binary-search-tree/)

- 简单

给你一个含重复值的二叉搜索树（BST）的根节点 `root` ，找出并返回 BST 中的所有 [众数](https://baike.baidu.com/item/众数/44796)（即，出现频率最高的元素）。

如果树中有不止一个众数，可以按 **任意顺序** 返回。

假定 BST 满足如下定义：

- 结点左子树中所含节点的值 **小于等于** 当前节点的值
- 结点右子树中所含节点的值 **大于等于** 当前节点的值
- 左子树和右子树都是二叉搜索树

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2021/03/11/mode-tree.jpg)

```
输入：root = [1,null,2,2]
输出：[2]
```

**示例 2：**

```
输入：root = [0]
输出：[0]
```

**提示：**

- 树中节点的数目在范围 `[1, 104]` 内
- `-10^5 <= Node.val <= 10^5`

**进阶：**你可以不使用额外的空间吗？（假设由递归产生的隐式调用栈的开销不被计算在内）

**解法一：中序遍历+哈希**

中序遍历得到数：频率的映射，然后根据频率排序，输出结果即可

**解法二：中序遍历+动态比较**

不保存遍历结果，只动态比较，当前元素`base`的频率`count`和历史最大频率`maxCnt`相比，如果一样大，就把当前元素加入`ans`，如果更大，就把`ans`清空，并把`base`加入，并且把`maxCnt`赋为当前`count`

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    base, count, maxCnt = 0, 0, 0
    ans = []
    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        self.traversal(root)
        return self.ans

    def traversal(self, root):
        if not root:
            return 
        self.traversal(root.left)
        self.update(root.val)
        self.traversal(root.right)

    def update(self, x):
        if x == self.base:
            self.count += 1
        else:
            self.base = x
            self.count = 1
        if self.count == self.maxCnt:
            self.ans.append(x)
        elif self.count > self.maxCnt:
            self.ans.clear()
            self.ans.append(x)
            self.maxCnt = self.count
```

**解法三：迭代的中序遍历**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        stack = []
        cur = root
        pre = None
        count, maxCnt = 0, 0
        ans = []

        while stack or cur:
            if cur:
                stack.append(cur)
                cur = cur.left
            else:
                cur = stack.pop()
                if not pre:
                    count = 1
                elif pre.val == cur.val:
                    count += 1
                else:
                    count = 1
                
                if count == maxCnt:
                    ans.append(cur.val)
                elif count > maxCnt:
                    ans.clear()
                    ans.append(cur.val)
                    maxCnt = count
                pre = cur
                cur = cur.right

        return ans
```

