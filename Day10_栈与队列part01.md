

[TOC]



## [232.用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)

```python
class MyQueue:

    def __init__(self):
        """用一个输入栈和一个输出栈来实现队列"""
        self.stack_in = []
        self.stack_out = []
        # 用list来模拟栈
        # pop 移除列表中的一个元素（默认最后一个元素），并且返回该元素的值: 模拟出栈
        # append 在列表末尾添加新的对象: 模拟入栈

        
    def push(self, x: int) -> None:
        self.stack_in.append(x)
        

    def pop(self) -> int:
        
        # 空队列直接返回none
        if self.empty():
            return None

        if self.stack_out:
            # 输出栈不为空, 直接弹出输出栈栈顶元素
            return self.stack_out.pop()
            
        else:
            # 输出栈空, 先把输入栈内的元素逐个压入输出栈, 再弹出输出栈栈顶元素
            stack_in_length = len(self.stack_in)
            for i in range(stack_in_length):
                elem = self.stack_in.pop() 
                self.stack_out.append(elem)
            return self.stack_out.pop()


    def peek(self) -> int:
        result = self.pop() # 取出队列开头元素
        self.stack_out.append(result) # 恢复队列原样
        return result
        

    def empty(self) -> bool:
        return not (self.stack_in or self.stack_out)
        


# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```

## [225.用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)

```python
class MyStack:

    def __init__(self):
        self.queue_main = []
        self.queue_aux = []
        # 用列表模拟队列
        # insert(0, elem) 将elem插入列表 模拟入队
        # pop() 移除列表中最后一个元素，并且返回该元素的值, 模拟出队
        

    def push(self, x: int) -> None:
        self.queue_main.insert(0, x)
        

    def pop(self) -> int:

        if self.empty():
            return None
        
        # 把主队列除队尾元素(栈顶元素)外备份到辅队列
        queue_size = len(self.queue_main)
        for i in range(queue_size - 1):
            queue_peek = self.queue_main.pop()
            self.queue_aux.insert(0, queue_peek)
        
        #交换主队列与辅队列
        self.queue_main, self.queue_aux = self.queue_aux, self.queue_main

        # 弹出辅助队列队头元素(栈顶元素出栈)
        return self.queue_aux.pop()

        

    def top(self) -> int:
        result = self.pop() # 先弹出栈顶元素
        self.push(result) # 再把栈顶元素入栈
        return result
        

    def empty(self) -> bool:
        return len(self.queue_main) == 0
        


# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```



## [20.有效的括号](https://leetcode.cn/problems/valid-parentheses/)

### 思路

括号的"后开先闭"特性（如 `({[]})` 中 `]` 需先匹配 `[`）与栈的 LIFO（后进先出）行为完全契合，自然联想到用栈跟踪未闭合的括号。

使用栈存储每个左括号对应的期望右括号，遇到右括号时检查是否与栈顶匹配，匹配则弹出栈顶，否则无效；最终栈空则所有括号有效闭合。

### 代码实现

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []  # 初始化栈，用于存储期望的闭合括号
        
        for char in s:  # 遍历字符串中的每个字符
            # 遇到左括号时，将其对应的右括号入栈（记录期望的闭合符）
            if char == "(":
                stack.append(")")
            elif char == "{":
                stack.append("}")
            elif char == "[":
                stack.append("]")
            
            # 遇到右括号时的处理
            elif not stack or stack[-1] != char:
                # 情况1：栈为空（右括号多余）-> 无效
                # 情况2：栈顶不匹配（括号交叉）-> 无效
                return False
            else:
                # 栈顶匹配成功，弹出当前闭合的括号
                stack.pop()
        
        # 最终栈空说明所有括号有效闭合
        # 栈非空说明有未闭合的左括号
        return not stack
```




## [1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)

### 思路1

乍一看，涉及到移除字符数组的元素，昨天刚总结过的双指针法来小试一下，快指针遍历字符，慢指针维护有效序列，遇到重复时回退慢指针实现相邻重复项的删除。

- **时间复杂度**：O(n)，快指针完整扫描字符串一次
- **空间复杂度**：O(n)，字符串转为列表的空间消耗（Python 字符串不可变）

### 代码1

```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        slow, fast = 0, 0  # 初始化双指针：slow维护有效序列边界，fast扫描全串
        s_list = list(s)   # 字符串不可变→转为列表方便原地修改
        
        while fast < len(s_list):  # 快指针遍历整个字符串
            # 核心逻辑：将快指针字符复制到慢指针位置（覆盖写入）
            s_list[slow] = s_list[fast]  
            
            # 检查相邻重复项（需确保slow>0才有前一字符）
            if slow > 0 and s_list[slow] == s_list[slow-1]:
                slow -= 1  # 回退慢指针：模拟"删除"重复项（后续写入会覆盖）
            else:
                slow += 1  # 无重复项：慢指针前进（扩展有效序列）
            
            fast += 1  # 快指针始终前移
        
        # 有效序列为 s_list[0:slow]（slow指向下一个写入位）
        return "".join(s_list[:slow])  
```

### 思路2

既然今天的主题是栈，消除相邻重复项的本质是"后进先出"的匹配过程（类似括号匹配），遇到相同字符需要立即消除最近添加的字符，栈的 LIFO 特性完美契合此需求。**使用栈存储非重复字符，遍历时比较当前字符与栈顶元素，相同则弹出（消除重复），否则压入栈中，最终栈中元素即为去重结果。**

- **时间复杂度**：O(n)，仅需遍历字符串一次，每个字符最多入栈/出栈一次
- **空间复杂度**：O(n)，栈空间占用（最坏情况下无重复字符需存储整个字符串）

### 代码2

```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        stack = []  # 初始化空栈，存储非重复字符
        
        for char in s:  # 遍历字符串中的每个字符
            # 核心逻辑：检查当前字符是否与栈顶相同
            if not stack or stack[-1] != char:  
                # 情况1：栈为空（无比较对象）
                # 情况2：当前字符≠栈顶字符（无重复）
                stack.append(char)  # 字符入栈
            else:
                # 当前字符==栈顶字符（出现相邻重复）
                stack.pop()  # 弹出栈顶元素（消除重复对）
        
        return "".join(stack)  # 栈中剩余字符拼接为结果字符串
```

