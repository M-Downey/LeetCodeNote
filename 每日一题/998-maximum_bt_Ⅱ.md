### [998. 最大二叉树 II](https://leetcode.cn/problems/maximum-binary-tree-ii/)

- 中等

**最大树** 定义：一棵树，并满足：其中每个节点的值都大于其子树中的任何其他值。

给你最大树的根节点 `root` 和一个整数 `val` 。

就像 [之前的问题](https://leetcode.cn/problems/maximum-binary-tree/) 那样，给定的树是利用 `Construct(a)` 例程从列表 `a`（`root = Construct(a)`）递归地构建的：

- 如果 `a` 为空，返回 `null` 。
- 否则，令 `a[i]` 作为 `a` 的最大元素。创建一个值为 `a[i]` 的根节点 `root` 。

- `root` 的左子树将被构建为 `Construct([a[0], a[1], ..., a[i - 1]])` 。

- `root` 的右子树将被构建为 `Construct([a[i + 1], a[i + 2], ..., a[a.length - 1]])` 。

- 返回 `root` 。

请注意，题目没有直接给出 `a` ，只是给出一个根节点 `root = Construct(a)` 。

假设 `b` 是 `a` 的副本，并在末尾附加值 `val`。题目数据保证 `b` 中的值互不相同。

返回 `Construct(b)` 。

**示例 1：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/23/maximum-binary-tree-1-2.png)

```
输入：root = [4,1,3,null,null,2], val = 5
输出：[5,4,null,1,3,null,null,2]
解释：a = [1,4,2,3], b = [1,4,2,3,5]
```

**示例 2：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/23/maximum-binary-tree-2-2.png)

```
输入：root = [5,2,4,null,1], val = 3
输出：[5,2,4,null,1,null,3]
解释：a = [2,1,5,4], b = [2,1,5,4,3]
```

**示例 3：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/23/maximum-binary-tree-3-1.png)

```
输入：root = [5,2,3,null,1], val = 4
输出：[5,2,4,null,1,3]
解释：a = [2,1,5,3], b = [2,1,5,3,4]
```

**提示：**

- 树中节点数目在范围 `[1, 100]` 内
- `1 <= Node.val <= 100`

- 树中的所有值 **互不相同**
- `1 <= val <= 100`

**解法一：双指针**

从数组构建最大二叉树的方法：递归，找到数组最大值作为分割点，然后递归构建左子树和右子树

在最大树对应的数组最后一位添加一个元素对应的最大树，这个节点一定**插在右子树上**，因为它在原数组的最后

所以只需去找当前节点的右子树，找到第一个比**插入值更小的节点**，就要插在它上面，子树一定插在当前节点的左子树（因为数组中在它左边），而插入节点一定是上一个节点的右孩子

用双指针遍历， `pre` 和 `cur` ，找到第一个小于插入值的节点，然后插入节点即可

如果没有找到，说明不是要插入在树中，要插在叶子节点上，最后为 `pre` 加右孩子即可

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    # 最后一个节点插入后，一定在右子树上
    def insertIntoMaxTree(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        pre, cur = None, root
        while cur:
            if val > cur.val:
                # 如果 val 大于根节点，直接放到左子树就行
                if not pre:
                    return TreeNode(val, root, None)
                # 否则把当前节点放到左子树，新节点放到 pre 的右子树
                node = TreeNode(val, cur, None)
                pre.right = node
                return root
            else:
                pre = cur
                cur = cur.right
        # 如果没有插到树中间，就是遍历到叶子节点了
        pre.right = TreeNode(val)
        return root
```

