[TOC]

## [150.逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)



### 思路

逆波兰表达式（后缀表达式）的计算使用栈结构处理：遍历表达式中的每个元素：

- 遇到数字则压入栈中。
- 遇到运算符则从栈中弹出两个数字（先弹出的为右操作数，后弹出的为左操作数），进行运算后将结果压回栈中。
- 除法运算需向零取整（直接使用 `int(a/b)` 实现）。
- 表达式遍历完成后，栈中剩余的唯一元素即为计算结果。

### 复杂度

- **时间复杂度**：O(n)，遍历每个元素一次，栈操作均为 O(1)。
- **空间复杂度**：O(n)，栈的空间占用。

### 代码

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        for token in tokens:
            if token in ["+", "-", "*", "/"]:  # 遇到运算符
                b = stack.pop()  # 弹出右操作数
                a = stack.pop()  # 弹出左操作数
                if token == '+':
                    stack.append(a + b)
                elif token == '-':
                    stack.append(a - b)
                elif token == '*':
                    stack.append(a * b)
                else:  # 除法使用 int(a/b) 向零取整
                    stack.append(int(a / b))
            else:  # 遇到数字，转换为整数后压栈
                stack.append(int(token))
        return stack.pop()  # 栈中最后元素即为结果
```



## [239.滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

### 思路

使用单调队列（双端队列实现）高效求解滑动窗口最大值问题：

1. 单调队列设计：维护一个从大到小的单调队列（队头最大）
   - `push`操作：插入新元素时，从队尾开始移除所有小于当前值的元素（确保队列单调性）
   - `pop`操作：只有当窗口移出的元素等于队头时才移除队头（说明当前最大值已离开窗口）
   - `front`操作：直接返回队头元素（即当前窗口最大值）
2. 滑动窗口处理：
   - 初始化：将前k个元素加入单调队列
   - 滑动窗口：每次移动时：
     - 弹出离开窗口的元素（如果它是当前最大值）
     - 将新元素加入单调队列
     - 记录当前窗口最大值（队列头部）

### 复杂度

- **时间复杂度**：O(n) - 每个元素最多入队/出队一次
- **空间复杂度**：O(k) - 队列最多存储k个元素

```python
from collections import deque

class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        que = MyQueue()  # 单调队列实例
        result = []      # 存储结果
        
        # 初始化第一个窗口
        for i in range(k):
            que.push(nums[i])
        result.append(que.front())  # 记录第一个窗口最大值
        
        # 滑动窗口处理后续元素
        for i in range(k, len(nums)):
            # 移出窗口左边的元素（仅当它是当前最大值时才从队列移除）
            que.pop(nums[i - k])
            # 添加新元素到窗口右边
            que.push(nums[i])
            # 记录当前窗口最大值
            result.append(que.front())
        
        return result

class MyQueue:
    def __init__(self):
        # 使用双端队列实现单调队列（存储实际数值）
        self.queue = deque()
    
    def pop(self, value):
        """移除指定值（仅当该值是队列头部时才移除）"""
        if self.queue and value == self.queue[0]:
            self.queue.popleft()  # 最大值离开窗口时移除
    
    def push(self, value):
        """添加新值并维护单调性"""
        # 从队尾移除所有小于当前值的元素（维护从大到小的单调性）
        while self.queue and value > self.queue[-1]:
            self.queue.pop()
        # 将当前值加入队尾
        self.queue.append(value)
    
    def front(self):
        """获取当前队列最大值（即队头元素）"""
        return self.queue[0]
```

### 再思考

这个问题的核心在于滑动窗口最大值的高效计算。直接对每个窗口遍历求最大值的时间复杂度为O(n*k)，在n大时会超时。因此需要优化。

滑动窗口每次只移动一个位置，相邻窗口有重叠。如果每次重新扫描整个窗口，会重复计算。我们希望利用重叠部分的已有信息。

而使用单调队列队存储可能成为窗口最大值的元素（且按单调递减排列），可以在O(1)时间内获取当前窗口的最大值，并且每个元素进入和离开队列各一次，均摊O(1)操作。

**单调队列不是基础数据结构**，它属于一种优化技巧，需要一定的经验积累。

## [347.前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)

