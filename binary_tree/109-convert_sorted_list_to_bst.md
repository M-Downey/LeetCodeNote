### [109. 有序链表转换二叉搜索树](https://leetcode.cn/problems/convert-sorted-list-to-binary-search-tree/)

- 中等

给定一个单链表的头节点  `head` ，其中的元素 **按升序排序** ，将其转换为高度平衡的二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树*每个节点* 的左右两个子树的高度差不超过 1。

**示例 1:**

 ![img](https://assets.leetcode.com/uploads/2020/08/17/linked.jpg)

```
输入: head = [-10,-3,0,5,9]
输出: [0,-3,9,-10,null,5]
解释: 一个可能的答案是[0，-3,9，-10,null,5]，它表示所示的高度平衡的二叉搜索树。
```

**示例 2:**

```
输入: head = []
输出: []
```

**提示:**

- `head` 中的节点数在`[0, 2 * 104]` 范围内
- `-105 <= Node.val <= 105`

#### 解法一：分治

**思路：**

1. 获取链表中的中间结点作为根节点，然后递归构造左子树和右子树

2. 获取链表中间结点的思路：快慢指针法，`fast`指针一次移动2下，`slow`指针移动1下，直到`fast`指针指向`right`（奇数个元素）或`fast.next`指向`right`（偶数个元素）

**注意：**

1. 具体地，设当前链表的左端点为`left`，右端点`right`，包含关系为「左闭右开」，即`left`包含在链表中而`right`不包含在链表中。我们希望快速地找出链表的中位数节点`mid`。

2. 为什么要设定「左闭右开」的关系?由于题目中给定的链表为单向链表，访问后继元素十分容易，但无法直接访问前驱元素。因此在找出链表的中位数节点`mid`之后，如果设定「左闭右开!的关系，我们就可以直接用`(left, mid)`以及`(mid.neat, right)`来表示左右子树对应的列表了。并且，初始的列表也可以用`(head, null)`方便地进行表示，其中 `null`表示空节点。

   

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedListToBST(self, head: Optional[ListNode]) -> Optional[TreeNode]:
        def getMedian(left: ListNode, right: ListNode) -> ListNode:
            fast = slow = left
            while fast != right and fast.next != right:
                fast = fast.next.next
                slow = slow.next
            return slow
        def buildTree(left: ListNode, right: ListNode) -> TreeNode:
            if left == right:
                return None
            mid = getMedian(left, right)
            root = TreeNode(mid.val)
            root.left = buildTree(left, mid)
            root.right = buildTree(mid.next, right)
            return root
        return buildTree(head, None)
```

