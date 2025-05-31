
## [24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/description/)

### 思路

要两两交换链表中的相邻节点，可以使用迭代法遍历链表并交换相邻节点。关键步骤包括：

1. 虚拟头节点：创建虚拟头节点简化操作，处理头节点交换的情况。
2. 交换相邻节点：
    - 保存当前节点对的前节点（tmp1）和下一节点对的头节点（tmp3）。
    - 调整指针：当前节点指向后节点，后节点指向前节点，前节点指向下一节点对的头节点。
3. 移动指针：将当前指针后移两位，继续处理下一对节点。
### 复杂度分析
- 时间复杂度：O(n)，其中 n 是链表节点数。遍历链表一次。
- 空间复杂度：O(1)，仅使用常数空间。
### 代码
```Python
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy_head = ListNode(next=head)
        
        cur = dummy_head

        while(cur.next and cur.next.next):

            tmp1 = cur.next # 临时保存左节点
            tmp3 = cur.next.next.next # 临时保存下一组的左节点

            cur.next = cur.next.next # 虚拟头节点指向右节点
            cur.next.next = tmp1  # 右节点指向左节点
            tmp1.next =tmp3 # 左节点指向下一组的左节点

            cur = cur.next.next # 后移两位

        return dummy_head.next
```

## [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

### 思路
    
要删除链表的倒数第 N 个节点，关键是如何定位到该节点的前一个节点。使用快慢指针可以高效解决：

1. 虚拟头节点：创建dummy_head 指向实际头节点，简化删除头节点的操作。
2. 快指针先行：快指针先移动 N+1 步，使快慢指针之间保持 N+1 个节点的距离。
3. 同步移动：同时移动快慢指针，当快指针到达链表末尾时，慢指针指向倒数第 N+1 个节点（即待删除节点的前一个节点）。
4. 删除节点：修改慢指针的 next 指针，跳过待删除节点。

### 代码
```Python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        """使用快慢指针"""
        
        dummy_head = ListNode(next=head)

        fast = dummy_head
        slow = dummy_head

        # 先把快指针移动n+1步
        for _ in range(n+1):
            fast = fast.next
        
        # 再同时移动快慢指针
        while(fast):
            fast = fast.next
            slow = slow.next
        
        # 此时slow的下一个节点是要被删除的
        slow.next = slow.next.next

        return dummy_head.next
```

## [面试题 02.07. 链表相交](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/description/)

### 方法1：双指针（长度差法）

#### 思路
要找到两个链表的相交节点，可以先计算两个链表的长度差。然后让较长的链表先走长度差步，使得两个链表剩余长度相同。接着同时遍历两个链表，当遇到相同的节点时即为相交节点。

#### 算法步骤
1. **计算长度**：遍历两个链表，分别计算长度 `lenA` 和 `lenB`。
2. **对齐起点**：将较长链表的头指针向后移动 `|lenA - lenB|` 步，使两个链表剩余长度相同。
3. **同时遍历**：同时遍历两个链表，比较节点是否相同。若相同则返回该节点；若遍历结束未找到，则返回 `None`。

#### 复杂度分析
- **时间复杂度**：O(m+n)，其中 m 和 n 分别为两个链表的长度。需要遍历两个链表各两次（一次计算长度，一次找交点）。
- **空间复杂度**：O(1)，仅使用常数空间。

#### 代码

```Python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        lenA, lenB = 0, 0
        cur = headA
        while cur:         # 求链表A的长度
            cur = cur.next 
            lenA += 1
        cur = headB 
        while cur:         # 求链表B的长度
            cur = cur.next 
            lenB += 1
        curA, curB = headA, headB
        if lenA > lenB:     # 让curB为最长链表的头，lenB为其长度
            curA, curB = curB, curA
            lenA, lenB = lenB, lenA 
        for _ in range(lenB - lenA):  # 让curA和curB在同一起点上（末尾位置对齐）
            curB = curB.next 
        while curA:         #  遍历curA 和 curB，遇到相同则直接返回
            if curA == curB:
                return curA
            else:
                curA = curA.next 
                curB = curB.next
        return None 
```

### 方法二：双指针（拼接法）
#### 思路
    另一种巧妙的方法是将两个链表拼接起来。
    指针 pA 从链表A的头节点开始遍历，遍历完A后接着遍历B。
    指针 pB 从链表B的头节点开始遍历，遍历完B后接着遍历A。
    当 pA 和 pB 相遇时，即为相交节点（因为两个指针都走了相同的步数：A+B）。
#### 算法步骤
    初始化指针 pA = headA, pB = headB。
    遍历：
        若 pA 不为空，则移动到下一个节点；若为空，则跳到链表B的头节点。
        若 pB 不为空，则移动到下一个节点；若为空，则跳到链表A的头节点。
        当 pA == pB 时，返回该节点。
#### 复杂度分析
时间复杂度：O(m+n)，其中 m 和 n 分别为两个链表的长度。
空间复杂度：O(1)。
#### 代码
```Python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        pA, pB = headA, headB
        while pA != pB:
            pA = pA.next if pA else headB
            pB = pB.next if pB else headA
        return pA
```


