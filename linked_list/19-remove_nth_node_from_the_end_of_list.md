### [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

- 中等

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

**示例 2：**

```
输入：head = [1], n = 1
输出：[]
```

**示例 3：**

```
输入：head = [1,2], n = 1
输出：[1]
```

**提示：**

- 链表中结点的数目为 `sz`
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

**进阶：**你能尝试使用一趟扫描实现吗？

**解法一：**

先遍历一遍求长度`len`，再遍历到第`n-len+1`个节点并删除，注意`n=len`的时候，即删除第一个节点，处理不同，可以用`dummy`，也可以直接返回`head.next`

**解法二：双指针**

设想：`head=[1, 2, 3, 4, 5]`，`n=2`的情况

先让`q`走`n`步，`p`再跟着走，直到`q.next=None`时停止，此时`p`指向倒数第`n+1`个节点，要删除它后面的

同样要注意删除的是第1个节点，即`q`被遍历到尾节点，此时`q.next=None`直接返回`head.next`

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        p, q = head, head
        while n:
            q = q.next
            n -= 1
        if not q:
            return head.next
        while q.next:
            p = p.next
            q = q.next
        p.next = p.next.next
        return head
```

