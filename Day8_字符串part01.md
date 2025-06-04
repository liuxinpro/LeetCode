Day8 字符串
## [344. 反转字符串](https://leetcode.cn/problems/reverse-string/)
### 思路 
使用双指针，一个指向开头，一个指向末尾，然后交换两个指针所指的字符，同时向中间移动，直到两个指针相遇。
### 代码

```Python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left = 0
        right = len(s) -1

        while left < right:
            tmp = s[left]
            s[left] = s[right]
            s[right] = tmp
            left += 1
            right -= 1
```

## [541. 反转字符串 II](https://leetcode.cn/problems/reverse-string-ii/description/)

### 思路
我们按照2k的步长遍历字符串，对于每个2k的块，反转前k个字符。注意处理边界情况（当剩余字符不足k时，全部反转；如果大于等于k但小于2k，则反转前k个）

### 代码
```Python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        i = 0
        s_list = list(s)
        n = len(s)
        for i in range(0, n-1, 2*k):
            # 反转从i开始的k个字符，如果不足k个，则反转所有剩余的
            left = i
            right = min(i+k-1, n-1)
            while left < right:
                s_list[left], s_list[right] = s_list[right], s_list[left]
                left += 1
                right -= 1
        
        s = "".join(s_list)
        return s
```

## [54. 替换数字（第八期模拟笔试）](https://kamacoder.com/problempage.php?pid=1064)

### 思路
遍历字符串，遇到数字字符（'0'到'9'）就替换为"number"。

### 代码
```Python
class Solution(object):
    def replaceNumber(self, s):

        s_list = list(s)

        for i, c in enumerate(s_list):
            if ord("0") <= ord(c) <= ord("9"):
                s_list[i] = "number"
        
        return "".join(s_list)
       
if __name__ == "__main__":
    solution = Solution()

    while True:
        try:
            s = input()
            result = solution.replaceNumber(s)
            print(result)
        except EOFError:
            break

```
## 今日总结

题目很简单 一个小时弄完所有



