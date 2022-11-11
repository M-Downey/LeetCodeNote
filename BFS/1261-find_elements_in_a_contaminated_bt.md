### [1261. 在受污染的二叉树中查找元素](https://leetcode.cn/problems/find-elements-in-a-contaminated-binary-tree/)

- 中等

给出一个满足下述规则的二叉树：

1. `root.val == 0`
2. 如果 `treeNode.val == x` 且 `treeNode.left != null`，那么 `treeNode.left.val == 2 * x + 1`
3. 如果 `treeNode.val == x` 且 `treeNode.right != null`，那么 `treeNode.right.val == 2 * x + 2`

现在这个二叉树受到「污染」，所有的 `treeNode.val` 都变成了 `-1`。

请你先还原二叉树，然后实现 `FindElements` 类：

- `FindElements(TreeNode* root)` 用受污染的二叉树初始化对象，你需要先把它还原。
- `bool find(int target)` 判断目标值 `target` 是否存在于还原后的二叉树中并返回结果。

**示例 1：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/16/untitled-diagram-4-1.jpg)

```
输入：
["FindElements","find","find"]
[[[-1,null,-1]],[1],[2]]
输出：
[null,false,true]
解释：
FindElements findElements = new FindElements([-1,null,-1]); 
findElements.find(1); // return False 
findElements.find(2); // return True 
```

**示例 2：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/16/untitled-diagram-4.jpg)

```
输入：
["FindElements","find","find","find"]
[[[-1,-1,-1,-1,-1]],[1],[3],[5]]
输出：
[null,true,true,false]
解释：
FindElements findElements = new FindElements([-1,-1,-1,-1,-1]);
findElements.find(1); // return True
findElements.find(3); // return True
findElements.find(5); // return False
```

**示例 3：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/11/16/untitled-diagram-4-1-1.jpg)

```
输入：
["FindElements","find","find","find","find"]
[[[-1,null,-1,-1,null,-1]],[2],[3],[4],[5]]
输出：
[null,true,false,false,true]
解释：
FindElements findElements = new FindElements([-1,null,-1,-1,null,-1]);
findElements.find(2); // return True
findElements.find(3); // return False
findElements.find(4); // return False
findElements.find(5); // return True
```

**提示：**

- `TreeNode.val == -1`
- 二叉树的高度不超过 `20`
- 节点的总数在 `[1, 10^4]` 之间
- 调用 `find()` 的总次数在 `[1, 10^4]` 之间
- `0 <= target <= 10^6`

**解法一：BFS**

`BFS` 更新树中结点值，并用哈希表记录所有节点值，用于查找

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class FindElements:

    def __init__(self, root: Optional[TreeNode]):
        q = [root]
        root.val = 0
        self.val = [0]
        while q:
            node = q.pop()
            if node.left:
                node.left.val = 2 * node.val + 1
                q.append(node.left)
                self.val.append(node.left.val)
            if node.right:
                node.right.val = 2 * node.val + 2
                q.append(node.right)
                self.val.append(node.right.val)
        

    def find(self, target: int) -> bool:
        return target in self.val


# Your FindElements object will be instantiated and called as such:
# obj = FindElements(root)
# param_1 = obj.find(target)
```

**解法二：DFS**

`DFS` 更新树中结点值，并用哈希表记录所有节点值，用于查找

自顶向下地更新值

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class FindElements:

    def __init__(self, root: Optional[TreeNode]):
        q = [root]
        self.val = []
        def dfs(root, val):
            if not root:
                return
            root.val = val
            self.val.append(val)
            dfs(root.left, 2 * val + 1)
            dfs(root.right, 2 * val + 2)
        dfs(root, 0)
        

    def find(self, target: int) -> bool:
        return target in self.val


# Your FindElements object will be instantiated and called as such:
# obj = FindElements(root)
# param_1 = obj.find(target)
```

