### [429. N 叉树的层序遍历](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/)

- 中等

给定一个 N 叉树，返回其节点值的*层序遍历*。（即从左到右，逐层遍历）。

树的序列化输入是用层序遍历，每组子节点都由 null 值分隔（参见示例）。

**示例 1：**

 ![img](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

```
输入：root = [1,null,3,2,4,null,5,6]
输出：[[1],[3,2,4],[5,6]]
```

**示例 2：**

 ![img](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)

```
输入：root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
输出：[[1],[2,3,4,5],[6,7,8,9,10],[11,12,13],[14]]
```

**提示：**

- 树的高度不会超过 `1000`
- 树的节点总数在 `[0, 10^4]` 之间

**解法一：队列**

队列中保存一层节点

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""

class Solution:
    def levelOrder(self, root: 'Node') -> List[List[int]]:
        # root.children是个节点列表
        if not root:
            return []
        q = [root]
        ans = []
        while q:
            ans.append([node.val for node in q])
            tmp = []
            for node in q:
                for child in node.children:
                    tmp.append(child)
            q = tmp
        return ans
```

java 实现

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> children;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, List<Node> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> res = new ArrayList();
        if(root == null){
            return res;
        }
        List<Node> q = new ArrayList();
        q.add(root);
        while(q != null && q.size() > 0){
            // vals 保存当前层的所有值 nodes保存下一层节点
            List<Integer> vals = new ArrayList();
            List<Node> nodes = new ArrayList();
            for(int i = 0; i < q.size(); i++){
                Node node = q.get(i);
                vals.add(node.val);
                // System.out.println(node.children);
                if(node.children != null && node.children.size() > 0){
                    for(int j = 0; j < node.children.size(); j++){
                        nodes.add(node.children.get(j));
                    }
                }
            }
            // System.out.println(nodes);
            res.add(vals);
            q = nodes;
        }
        return res;
    }
}
```

java 官方版

```java
class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        if (root == null) {
            return new ArrayList<List<Integer>>();
        }

        List<List<Integer>> ans = new ArrayList<List<Integer>>();
        Queue<Node> queue = new ArrayDeque<Node>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            // 获得当前层的节点数，level 保存当前层的所有 value
            int cnt = queue.size();
            List<Integer> level = new ArrayList<Integer>();
            for (int i = 0; i < cnt; ++i) {
                Node cur = queue.poll(); // poll 方法出队
                level.add(cur.val);
                for (Node child : cur.children) {
                    queue.offer(child); // offer 方法入队
                }
            }
            ans.add(level);
        }

        return ans;
    }
}
```

