## [203. 移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)

- 简单

给你一个链表的头节点 `head` 和一个整数 `val` ，请你删除链表中所有满足 `Node.val == val` 的节点，并返回 **新的头节点** 。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2021/03/06/removelinked-list.jpg)

```
输入：head = [1,2,6,3,4,5,6], val = 6
输出：[1,2,3,4,5]
```

**示例 2：**

```
输入：head = [], val = 1
输出：[]
```

**示例 3：**

```
输入：head = [7,7,7,7], val = 7
输出：[]
```

**提示：**

- 列表中的节点数目在范围 `[0, 104]` 内
- `1 <= Node.val <= 50`
- `0 <= val <= 50`

**解法一：循环遍历**

循环遍历链表，如果`val`相等，就删去，如果不等就下一个

注意到，头结点的删除和其他节点删除不一样，为了统一，添加一个`dummy`头结点指向`head`

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: ListNode, val: int) -> ListNode:
        if not head:
            return head
        dummy = ListNode()
        dummy.next = head
        pre, cur = dummy, head
        while cur:
            if cur.val == val:
                pre.next = cur.next
                cur = cur.next
            else:
                pre = cur
                cur = cur.next
        return dummy.next
```

