## [226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

递归交换每个节点的左右子树，从根节点开始逐层向下翻转。时间复杂度为 O(n)（每个节点访问一次），空间复杂度为 O(h)（递归栈深度，h 为树高，最坏情况 h=n）。树问题优先考虑递归，通过分治思想将问题分解为子树处理，注意递归终止条件和节点操作顺序。

```python
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        # 递归终止条件：当前节点为空时返回
        if not root:
            return None
        
        # 核心操作：交换当前节点的左右子树
        root.left, root.right = root.right, root.left
        
        # 递归翻转左子树
        self.invertTree(root.left)
        # 递归翻转右子树
        self.invertTree(root.right)
        
        # 返回翻转后的根节点
        return root
```

## [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

递归比较左右子树是否镜像对称（左子树的左节点与右子树的右节点匹配，左子树的右节点与右子树的左节点匹配）。时间复杂度 O(n)（每个节点访问一次），空间复杂度 O(h)（递归栈深度，h 为树高，最坏情况 h=n）。对称二叉树本质是判断左右子树镜像关系，通过成对比较镜像位置的节点，可以高效解决该问题。

```python
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        # 空树视为对称
        if not root:
            return True
        # 比较左右子树是否镜像对称
        return self.compare(root.left, root.right)
    
    def compare(self, left: TreeNode, right: TreeNode) -> bool:
        # 处理节点为空的情况：左右同时为空时对称
        if not left and not right:
            return True
        # 左右只有一个为空时不对称
        if not left or not right:
            return False
        # 节点值不相等时不对称
        if left.val != right.val:
            return Falseht.val:
            return False
        
        # !!!递归比较镜像位置
        # 1. 左子树的左节点 vs 右子树的右节点（外侧）
        # 2. 左子树的右节点 vs 右子树的左节点（内侧）
        outside = self.compare(left.left, right.right)
        inside = self.compare(left.right, right.left)
        isSame = outside and inside

        return isSame
```

## [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

递归计算左右子树的最大深度，当前节点深度为左右子树最大深度加一。

时间复杂度 O(n)（每个节点访问一次），空间复杂度 O(h)（递归栈深度，h 为树高，最坏情况 h=n）。

```python
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        # 空节点深度为0
        if not root:
            return 0
        
        # 递归计算左子树深度
        left_depth = self.maxDepth(root.left)
        # 递归计算右子树深度
        right_depth = self.maxDepth(root.right)
        
        # 当前节点深度 = 左右子树最大深度 + 1
        return max(left_depth, right_depth) + 1
```

## [111. 二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

递归计算左右子树最小深度，注意处理单子树缺失情况（非叶子节点需继续向下查找）。

时间复杂度 O(n)（每个节点访问一次），空间复杂度 O(h)（递归栈深度，h 为树高，最坏情况 h=n）。

```python
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        # 空节点深度为0
        if root is None:
            return 0
        
        # 递归计算左右子树深度
        leftDepth = self.minDepth(root.left)
        rightDepth = self.minDepth(root.right)
        
        # 处理左子树缺失情况：返回右子树深度+1（非叶子节点需继续向下）
        if root.left is None and root.right is not None:
            return 1 + rightDepth
        
        # 处理右子树缺失情况：返回左子树深度+1（非叶子节点需继续向下）
        if root.left is not None and root.right is None:
            return 1 + leftDepth
        
        # 左右子树均存在：取最小深度+1
        return 1 + min(leftDepth, rightDepth)
```

