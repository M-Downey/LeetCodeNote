### [146. LRU 缓存](https://leetcode.cn/problems/lru-cache/) 四星

- 中等

请你设计并实现一个满足 [LRU (最近最少使用) 缓存](https://baike.baidu.com/item/LRU) 约束的数据结构。

实现 `LRUCache` 类：

- `LRUCache(int capacity)` 以 **正整数** 作为容量 `capacity` 初始化 LRU 缓存
- `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
- `void put(int key, int value)` 如果关键字 `key` 已经存在，则变更其数据值 `value` ；如果不存在，则向缓存中插入该组 `key-value` 。如果插入操作导致关键字数量超过 `capacity` ，则应该 **逐出** 最久未使用的关键字。

函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。

 

**示例：**

```
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
```

 

**提示：**

- `1 <= capacity <= 3000`
- `0 <= key <= 10000`
- `0 <= value <= 105`
- 最多调用 `2 * 105` 次 `get` 和 `put`



**解法一：双向链表 + 哈希表**

```java
class LRUCache {
    class ListNode {
        int key;
        int val;
        ListNode prev;
        ListNode next;

        public ListNode() {}

        public ListNode(int key, int val) {
            this.key = key;
            this.val = val;
        }
    }

    Map<Integer, ListNode> hm = new HashMap<>();
    int size;
    int capacity;
    ListNode head;
    ListNode tail;
    public LRUCache(int capacity) {
        // hm 用于 get 元素
        // 双向链表用于维护元素顺序
        this.capacity = capacity;
        this.size = 0;
        head = new ListNode();
        tail = new ListNode();
        head.next = tail;
        tail.prev = head;
        hm = new HashMap<>();
    }
    
    public int get(int key) {
        ListNode node = hm.get(key);
        if (node == null) {
            return -1;
        }
        // 移动到最前面
        moveToHead(node);
        return node.val;
    }
    
    public void put(int key, int value) {
        ListNode node = hm.get(key);
        // 如果已经存在
        if (node != null) {
            node.val = value;
            moveToHead(node);
        } else {
            ListNode newNode = new ListNode(key, value);
            hm.put(key, newNode);
            addToHead(newNode);
            size++;
            // 如果 size 超出容量，删除尾部
            if (size > capacity) {
                hm.remove(tail.prev.key);
                removeNode(tail.prev);
                size--;
            }
        }
    }

    // 把当前节点移动到头部
    private void moveToHead(ListNode node) {
        removeNode(node);
        addToHead(node);
    }

    // 移除当前节点
    private void removeNode(ListNode node) {
        node.next.prev = node.prev;
        node.prev.next = node.next;
    }

    // 添加一个节点到头部
    private void addToHead(ListNode node) {
        head.next.prev = node;
        node.next = head.next;
        node.prev = head;
        head.next = node;
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

