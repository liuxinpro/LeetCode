## 二叉树

```python
# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```



## 二叉树的遍历（递归实现）

前中后序指的就是中间节点的位置

- 前序遍历：中左右
- 中序遍历：左中右
- 后序遍历：左右中

### [144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/)

```
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []

        def dfs(node):
            if node is None:
                return
            
            res.append(node.val)
            dfs(node.left)
            dfs(node.right)
        
        dfs(root)

        return res
        
```

### [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

```python
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []

        def dfs(node):
            if node is None:
                return
            
            dfs(node.left)
            res.append(node.val)
            dfs(node.right)
        
        dfs(root)

        return res
```

### [145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/)

```python
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []

        def dfs(node):
            if node is None:
                return
            dfs(node.left)
            dfs(node.right)
            res.append(node.val)

        
        dfs(root)

        return res
```

## 二叉树的遍历（迭代实现）

### [144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/)

```python
def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
    if not root:
        return []
    
    stack = [root]  # 初始化栈（根节点入栈）
    res = []
    
    while stack:
        node = stack.pop()      # 弹出栈顶元素
        res.append(node.val)    # 访问该节点
        
        # 右子节点先入栈（后处理）
        if node.right:
            stack.append(node.right)
            
        # 左子节点后入栈（先处理）
        if node.left:
            stack.append(node.left)
    
    return res
```

### [145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/)

```python
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        
        stack = [root]  # 初始化栈（根节点入栈）
        res = []
        
        while stack:
            node = stack.pop()      # 弹出栈顶元素
            res.append(node.val)    # 访问该节点
            
            # 左子树先入栈（后出栈）
            if node.left:
                stack.append(node.left)
            # 右子树后入栈（先出栈
            if node.right:
                stack.append(node.right)
                

        # 中右左 ==> 左右中
        return res[::-1]
```

### [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

1. 从根节点开始，将所有左子节点依次入栈，直到最左边的节点（此时cur为空）。
2. 然后开始出栈，每次出栈一个节点，访问它（将值加入结果），并将cur指向该节点的右子节点。
3. 如果右子节点存在，则对这个右子节点同样进行步骤1（即将其所有左子节点入栈）；如果不存在，则继续出栈下一个节点。

```python
def inorderTraversal(self, root: TreeNode) -> List[int]:
    if not root:
        return []
    res = []
    stack = []
    cur = root  # 当前节点指针

    while cur or stack:
        # 1. 深入左子树（直到最左端）
        if cur:
            stack.append(cur)
            cur = cur.left  # 指针移动到左子节点
        
        # 2. 回溯处理节点
        else:
            cur = stack.pop()      # 弹出栈顶节点
            res.append(cur.val)    # 访问该节点（根节点）
            cur = cur.right        # 转向右子树
    return res
```

## [102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)



层序遍历（Level Order Traversal）的思路来源于**广度优先搜索（BFS）算法**在树结构中的应用。核心思想是通过队列实现节点的层次化处理：

- **树结构的特性**：树是分层结构，根节点在第一层，子节点在下一层
- **队列的FIFO特性**：先进先出队列完美匹配层次遍历需求
- **分层处理的关键**：在每一层开始前记录当前队列长度（即该层节点数），确保只处理本层节点
- **子节点入队策略**：处理当前节点时将其子节点入队，但延迟到下一层处理

```python
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        """二叉树层序遍历（BFS实现）"""
        # 边界情况处理：空树直接返回空列表
        if not root:
            return []
        
        # 初始化双端队列（用于高效popleft操作）
        # 初始状态：队列中只有根节点（第一层）
        queue = deque([root])
        result = []  # 存储最终结果
        
        # 当队列非空时继续处理（还有未遍历的节点）
        while queue:
            # 创建当前层的值列表
            level_vals = []
            # 关键步骤：记录当前层节点数量（此时队列长度=本层节点数）
            level_size = len(queue)
            
            # 处理当前层所有节点（循环level_size次）
            for _ in range(level_size):
                # 从队列左侧弹出当前节点（FIFO原则）
                cur_node = queue.popleft()
                # 将当前节点值加入本层列表
                level_vals.append(cur_node.val)
                
                # 将左子节点加入队列（下一层节点）
                if cur_node.left:
                    queue.append(cur_node.left)
                # 将右子节点加入队列（下一层节点）
                if cur_node.right:
                    queue.append(cur_node.right)
            
            # 将当前层值列表加入最终结果
            result.append(level_vals)
        
        return result
```

