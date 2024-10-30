### [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

- 中等

给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 *如果链表无环，则返回 `null`。*

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos`来表示链表尾连接到链表中的位置（**索引从 0 开始**）。如果 `pos` 是 `-1`，则在该链表中没有环。**注意：`pos` 不作为参数进行传递**，仅仅是为了标识链表的实际情况。

**不允许修改** 链表。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

```
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

```
输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环。
```

**提示：**

- 链表中节点的数目范围在范围 `[0, 104]` 内
- `-105 <= Node.val <= 105`
- `pos` 的值为 `-1` 或者链表中的一个有效索引

**进阶：**你是否可以使用 `O(1)` 空间解决此题？

**解法一：哈希表**

用`hash set`记录已访问的节点，再每次更新节点时判断它是否出现在`set`中，如果出现则返回，否则没有环的话链表会遍历结束，就返回`None`

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode cur = head;
        Set<ListNode> set = new HashSet<>();
        while (cur != null) {
            if (set.contains(cur)) {
                return cur;
            }
            set.add(cur);
            cur = cur.next;
        }
        return null;
    }
}
```

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        ls = []
        p = head
        while p:
            ls.append(p)
            p = p.next
            if p in ls:
                return p
        return None
```

**解法二：快慢指针**

slow, fast 如果有环，fast一定会等于 slow，因为在环内的相对速度 fast 更快一定会追上 slow

关键是找到入环点，相遇点到入环点的距离加上 n 倍的环长，等于头到入环点的距离

所以同时遍历 head 和 slow 直到它们相等，就是入环点

```java
设到入环点的距离是 a
fast 和 slow 在环内走了 b 相遇，相遇点到入环点距离为 c
则fast走的距离是slow的两倍
2(a+b)=a+n(b+c)+b
a=(n-1)b+nc
a=(n-1)(b+c)+c
也就是：走到入环点的距离和相遇点到入环点的距离加上 n-1 倍的环长相等
所以让 head 和 slow 一直遍历即可
  /**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        // slow, fast 如果有环 fast一定会等于 slow，因为在环内的相对速度
        // 另外，相遇点到入环点的距离加上 n 倍的环长，等于头到入环点的距离
        // 所以同时遍历 head 和 slow 直到它们相等，就是入环点
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                while (head != slow) {
                    head = head.next;
                    slow = slow.next;
                }
                return slow;
            }
        }
        return null;
    }
}
```

