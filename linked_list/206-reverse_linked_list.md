### [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

- 简单

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```
输入：head = [1,2]
输出：[2,1]
```

**示例 3：**

```
输入：head = []
输出：[]
```

 **提示：**

- 链表中节点的数目范围是 `[0, 5000]`
- `-5000 <= Node.val <= 5000`

**解法一：双指针**

我们拿有示例中的链表来举例，如动画所示：

 ![img](https://tva1.sinaimg.cn/large/008eGmZEly1gnrf1oboupg30gy0c44qp.gif)

首先定义一个`cur`指针，指向头结点，再定义一个`pre`指针，初始化为`null`。

然后就要开始反转了，首先要把 `cur->next` 节点用`tmp`指针保存一下，也就是保存一下这个节点。

为什么要保存一下这个节点呢，因为接下来要改变 `cur->next `的指向了，将`cur->next `指向`pre `，此时已经反转了第一个节点了。

接下来，就是循环走如下代码逻辑了，继续移动`pre`和`cur`指针。

最后，`cur `指针已经指向了`null`，循环结束，链表也反转完毕了。 此时我们`return pre`指针就可以了，`pre`指针就指向了新的头结点。

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        cur = head   
        pre = None
        while cur:
            tmp = cur.next # 保存一下 cur的下一个节点，因为接下来要改变cur->next
            cur.next = pre #反转
            #更新pre、cur指针
            pre = cur
            cur = tmp
        return pre
```