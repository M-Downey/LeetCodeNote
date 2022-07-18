### [24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

- 中等

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

```
输入：head = [1,2,3,4]
输出：[2,1,4,3]
```

**示例 2：**

```
输入：head = []
输出：[]
```

**示例 3：**

```
输入：head = [1]
输出：[1]
```

**提示：**

- 链表中节点的数目在范围 `[0, 100]` 内
- `0 <= Node.val <= 100`

**解法一：类似于双指针**

思路：要考虑需要改变哪些指针的值，把两个节点看成一组的话，需要改变上一组最后一个节点的指针，故需要`pre`指向上一组最后一个节点，用`p`，`q`指向本组第一个第二个节点，还需改变`p.next = q.next`，`q.next = p`，然后再为`pre`赋值为`p`，为`p`赋值为`pre.next`，为`q`赋值为`p.next，注意由于p可能为None，故要判断`

考虑特殊情况，只有`0`个节点或`1`个节点，直接返回即可

最后答案返回的`head`应是第二个节点，所以最开始要给`head`赋值为`q`

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        # pre上组最后一个,p是当前第一个, q是第二个
        # pre.next = q
        # p.next = q.next
        # q.next = p
        # pre = p
        # p = pre.next
        # q = p.next if p else None
        if not head or not head.next:
            return head
        dummy = ListNode()
        dummy.next = head
        pre, p, q = dummy, head, head.next
        head = q
        while p and q:
            p.next = q.next
            q.next = p
            pre.next = q
            pre = p
            p = pre.next
            q = p.next if p else None
        return head
```

