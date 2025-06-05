

## [151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)


乍一看直接用内置切片和split和join就解决了，然而题解第一句话就说这样让本题目失去了意义，确实是这样哈

```Python

class Solution:
    
    def reverseWords(self, s: str) -> str:
        """解法1"""
        s = s[::-1]
        s = " ".join([word[::-1] for word in s.split()])    
        return s    

    def reverseWords(self, s: str) -> str:
        """解法2"""

        s_list = list(s)
        n = len(s_list)

        # 1, 移除多余的空格, 使用快慢指针
        slow, fast = 0, 0
        word_count = 0
        while fast < n:
            # 跳过空格
            if s_list[fast] == " ":
                fast += 1
                continue
            
            # 非首单词前加空格
            if word_count > 0:
                s_list[slow] = " "
                slow += 1
            
            # 复制整个单词
            while fast < n and s_list[fast] != " ":
                s_list[slow] = s_list[fast]
                slow += 1
                fast += 1
            word_count += 1
        
        # 有效部分长度为slow
        s_list = s_list[:slow]
        n = slow
        print(s_list)


        # 2，原地反转字符串
        i, j = 0, n-1
        while i < j:
            s_list[i], s_list[j] = s_list[j], s_list[i]
            i += 1
            j -= 1
        
        # 3，反转单词(一个单词就是字符串的一个区间)
        start = 0
        for end in range(n):
            # 遇到结尾或遇到空格反转
            if end == n-1 or s_list[end+1] == " ":
                i, j = start, end
                while i < j:
                    s_list[i], s_list[j] = s_list[j], s_list[i]
                    i += 1
                    j -= 1
                start = end + 2
        
        return "".join(s_list)
```




## [55. 右旋字符串（第八期模拟笔试）](http://kamacoder.com/problempage.php?pid=1065)

如果只是为了解题确实一行代码s[-k:] + s[:-k]就搞定了
但是看了题解之后确实，这个题就是上一个题目的简化版本应用，不需要我们处理空格，区间数量已知且只固定为2


```Python

k = int(input())
s = input()

# print(s[-k:] + s[:-k])

s = list(s)

def reverseString(s):
    n = len(s)
    i, j = 0, n-1
    while i < j:
        s[i], s[j] = s[j], s[i]
        i += 1
        j -= 1
    return s


s = reverseString(s)
s[:k] = reverseString(s[:k])
s[k:] = reverseString(s[k:])

print("".join(s))
```


## [28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)

这个题目考察KMP算法 看了一遍没有掌握 不过今天你没时间了
## [459.重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/description/)
这个题目更是难度飙升 暂时先跳过


## 字符串总结
参考https://programmercarl.com/%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%80%BB%E7%BB%93.html

## 双指针总结
参考https://programmercarl.com/%E5%8F%8C%E6%8C%87%E9%92%88%E6%80%BB%E7%BB%93.html

| 技巧类型 | 应用场景 | 时间复杂度	| 经典问题 |
|----|----|-----|----|
| 快慢指针 | 原地修改数组/字符串	|O(n)	|移除元素、去重、去空格|
| 左右指针 | 数组/字符串反转	|O(n)	|反转字符串、旋转字符串|
| 滑动窗口 | 子串/子数组问题	|O(n)	|最小覆盖子串、最长无重复子串|
| 前后指针 | 有序数组操作	|O(n)	|两数之和、三数之和|

