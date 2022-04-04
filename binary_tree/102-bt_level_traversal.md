### 102. 二叉树的层次遍历

给你二叉树的根节点 `root` ，返回其节点值的 **层序遍历** 。 （即逐层地，从左到右访问所有节点）。

 ![image-20220404213352632](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20220404213352632.png)a

```
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[9,20],[15,7]]
```

```
输入：root = [1]
输出：[[1]]
```

```
输入：root = []
输出：[]
```

#### 解法一：带有深度属性的BFS

**思路：**用队列进行深度优先遍历，每次访问一个队头结点，把它的孩子结点入队（要把深度加一），最后创建一个二维列表，内含**深度**个空列表，根据深度向其中添加元素

**注意点：**

1. 当前结点node的孩子入队时，要先**检查它的孩子是否为空**，非空才入队
2. 注意处理事例中**输入为空树**的情况，输出也为空[]
3. 创建输出列表时，注意深度要加1：

```python
# res[-1][1] + 1，因为要从0到res[-1][1]创建列表
for i in range(res[-1][1] + 1):
	real_res.append([])
```



```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        # 广度优先遍历
        if not root:
            return []
        queue = [(root, 0)]
        res = []
        while queue:
            node = queue.pop(0)
            if node[0].left:
                queue.append((node[0].left, node[1] + 1))
            if node[0].right:
                queue.append((node[0].right, node[1] + 1))
            res.append((node[0].val, node[1]))
        # print(res)
        real_res = []
        # print(res[-1][1])
        for i in range(res[-1][1] + 1):
            real_res.append([])
        # print(real_res)
        for node in res:
            real_res[node[1]].append(node[0])  
        # print(real_res)
        return real_res
```

#### 解法2：BFS每次将同一层次的元素出队

**思路：**还是用队列进行BFS，但每次将同一层次的元素同时出队（将其值作为列表加入到结果中），同时将此层次结点的所有子结点加入到队列中

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        if not root:
            return []
        queue = [root]
        res = []
        while queue:
            res.append([node.val for node in queue])
            tmp = []
            for node in queue:
                if node.left:
                    tmp.append(node.left)
                if node.right:
                    tmp.append(node.right)
            queue = tmp
        # print(res)
        return res
```

