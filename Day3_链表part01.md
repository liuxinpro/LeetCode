Day3 链表part01

 

## 203.移除链表元素  

题目链接: https://leetcode.cn/problems/remove-linked-list-elements/description/

### 使用原生链表

- 处理头节点：使用循环跳过所有值等于 val 的头节点
- 处理非头节点：遍历链表，遇到值等于 val 的节点时跳过
- 返回新头节点

```Python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        """使用原生链表"""
        
        # 删除头节点
        while head and head.val == val:
            head = head.next

        # 删除非头节点 用current遍历
        current = head
        while current and current.next:

            if current.next.val == val:
                # 找到了 则删除
                current.next = current.next.next
            else:
                # 没找到 去找笑一个
                current = current.next
        
        return head
```

### 使用虚拟头节点
- 创建虚拟头节点：指向真实头节点，避免单独处理头节点
- 遍历链表：遇到值等于 val 的节点时跳过
- 返回虚拟头的下一个节点（真实头节点）

```Python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        """使用虚拟头节点"""

        dummpy_head = ListNode(next=head)

        cur = dummpy_head

        # 遍历链表元素
        while cur.next:
            if cur.next.val == val:
                cur.next = cur.next.next
            else:
                cur = cur.next
        
        return dummpy_head.next     
```

### 我的心得

虚拟头节点技巧是链表问题的核心技巧之一，能统一处理头节点和非头节点的操作逻辑，避免复杂的边界条件判断，建议重点掌握。

## 707.设计链表  


题目链接: https://leetcode.cn/problems/design-linked-list/description/
 
```Python

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class MyLinkedList:
    def __init__(self):
        self.dummy_head = ListNode()
        self.size = 0

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
        
        current = self.dummy_head.next
        for i in range(index):
            current = current.next
            
        return current.val

    def addAtHead(self, val: int) -> None:
        self.dummy_head.next = ListNode(val, self.dummy_head.next)
        self.size += 1

    def addAtTail(self, val: int) -> None:
        current = self.dummy_head
        while current.next:
            current = current.next
        current.next = ListNode(val)
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:
            return
        
        current = self.dummy_head
        for i in range(index):
            current = current.next
        current.next = ListNode(val, current.next)
        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return
        
        current = self.dummy_head
        for i in range(index):
            current = current.next
        current.next = current.next.next
        self.size -= 1
```



## 206.反转链表 



题目链接：https://leetcode.cn/problems/reverse-linked-list/description/

反转链表 同时需要处理当前节点，上一个节点，下一个节点

使用双指针法可以解决

```Python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        """使用双指针法"""

        pre = None
        cur = head

        while(cur):
            
            temp = cur.next # 临时存储下一个节点
            cur.next = pre # 当前节点指向前一个节点（即反转方向）
            pre = cur # 向后移动pre
            cur = temp # 向后移动cur
        
        return pre
```