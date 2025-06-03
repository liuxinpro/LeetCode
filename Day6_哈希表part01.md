Day6 哈希表

## [242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

### 思路
字母异位词的核心特征是两个字符串中每个字符的出现次数完全相同（仅顺序不同）。由于题目限定字符串仅包含小写字母（共26个），因此可以用长度为26的数组模拟哈希表，统计每个字符的出现频率。



### 代码
```Python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        record = [0] * 26 # 仅包含小写字母
        for i in s:
            record[ord(i) - ord("a")] += 1
        for i in t:
            record[ord(i) - ord("a")] -= 1
        
        for i in range(26):
            if record[i] != 0:
                return False
        else:
            return True
```

## [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/description/)

### 思路
题目要求找到两个数组的**交集**（结果中每个元素唯一，顺序无关）。利用 **集合（Set）** 的天然特性可高效解决问题：

### 代码
```Python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return list(set(nums1) & set(nums2))
```
## [202. 快乐数](https://leetcode.cn/problems/happy-number/)
### 思路
快乐数的核心判定逻辑基于 “平方和的迭代过程要么最终得到 1，要么进入无限循环（因为数字的平方和组合是有限的，必然重复）”。


```Python
class Solution:
    def isHappy(self, n: int) -> bool:
        # 用于记录迭代过程中出现过的数字，检测是否进入循环
        record = set()  
        # 当n已在record中时，说明进入循环，终止迭代
        while n not in record:  
            # 将当前数字n加入集合，标记为已访问（防止重复处理）
            record.add(n)  
            # 初始化新数的平方和为0
            new_num = 0  
            # 将数字n转为字符串，方便遍历每一位数字
            n_str = str(n)  
            # 遍历n的每一位字符（转为数字后计算平方和）
            for i in n_str:  
                # 累加当前位数字的平方到new_num
                new_num += int(i) ** 2  
            # 若平方和为1，说明是快乐数，直接返回True（提前终止优化）
            if new_num == 1:  
                return True
            # 否则，更新n为新的平方和，进入下一轮迭代
            else:  
                n = new_num
        # 若退出循环（n已在record中），说明进入循环，不是快乐数
        return False  
```

## [1. 两数之和](https://leetcode.cn/problems/two-sum/description/)
### 思路
两数之和的核心是高效找到“目标值与当前数的差值”是否已存在于之前的遍历中。暴力法（双重循环）时间复杂度为 O(n2)，而哈希表（字典） 可将查询操作的时间复杂度降为O(1)，从而优化整体效率。
### 代码
```Python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # 字典record：键为已遍历的数字，值为该数字对应的索引（用于快速查询）
        record = dict()  
        # 遍历数组nums，enumerate同时返回元素索引idx和值num
        for idx, num in enumerate(nums):  
            # 计算当前数字num需要的“互补数”：target - num
            complement = target - num  
            # 检查互补数是否在record中（即之前是否遍历过该互补数）
            if complement in record:  
                # 若存在，返回互补数的索引和当前数字的索引
                return [record[complement], idx]  
            # 若不存在，将当前数字和其索引存入字典，供后续数字查询
            record[num] = idx  
```

## 今日总结

哈希表是算法中“空间换时间”思想的经典体现，通过设计映射函数（如ASCII差值）将数据压缩存储，实现O(1)的查找效率。在解决存在性检测（互补数存在）、状态记录（字符计数）、等问题时，哈希结构能突破暴力解法的复杂度瓶颈，重构问题的时间维度。
- 有效的字母异位词（242）：使用长度为26的数组模拟哈希表，统计字符出现频率，通过频率匹配判断异位词。
- 两个数组的交集（349）：利用集合（Set）去重和求交集的特性，高效返回交集结果。
- 快乐数（202）：使用集合记录迭代过程中出现的数字，检测循环，判断是否为快乐数。
- 两数之和（1）：使用字典存储遍历过的数字及其索引，通过检查互补数（target - num）是否在字典中实现O(n)解法。
