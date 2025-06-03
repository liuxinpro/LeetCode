Day7 哈希表Part2

## [454. 四数相加 II](https://leetcode.cn/problems/4sum-ii/description/)

### 第一想法
最先想到暴力四重循环（O(n⁴)），但意识到肯定会超时。考虑过两两合并但没想清楚具体实现。

### 学习后思路
- 分组处理：将四个数组分成两组，nums1 和 nums2 为一组，nums3 和 nums4 为另一组。
- 预处理第一组：计算 nums1 和 nums2 中所有元素的两两之和，并用哈希表记录每个和出现的次数。
- 处理第二组：计算 nums3 和 nums4 中所有元素的两两之和，在哈希表中查找其相反数的出现次数并累加到结果中。
- 互补值计算：第二组的两数之和为 s 时，需在第一组中查找 -s 的出现次数。

### 代码
```Python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        # 初始化哈希表记录第一组的两数之和
        record = {}
        # 遍历nums1和nums2，计算所有两数之和
        for a in nums1:
            for b in nums2:
                total = a + b
                # 更新哈希表：存在则+1，不存在则初始化为1
                record[total] = record.get(total, 0) + 1
        
        count = 0  # 统计符合条件的四元组数量
        # 遍历nums3和nums4，计算所有两数之和
        for c in nums3:
            for d in nums4:
                total = c + d
                # 查找第一组中是否有互补值（-total）
                # 存在则累加次数，不存在则加0
                count += record.get(-total, 0)
        
        return count
```
### 复杂度分析
- 时间复杂度：需要分别遍历两组数组，每组进行双重循环，时间复杂度为O(n2)
- 空间复杂度：O(n2) 哈希表最多存储 n2个键值对（第一组的所有两数之和）。

## [383. 赎金信](https://leetcode.cn/problems/ransom-note/description/)

### 思路（第一想法直接就做出来了！开心）
1. **问题分析**：判断赎金信字符串 `ransomNote` 是否能由杂志字符串 `magazine` 中的字符组成。关键在于 `magazine` 中每个字符只能使用一次，且必须包含 `ransomNote` 的所有字符。
2. **核心思想**：使用计数法统计字符出现频率。先统计 `magazine` 中字符出现次数，再遍历 `ransomNote` 消耗字符计数。
3. **优化点**：在遍历赎金信时即时检查字符计数，一旦发现负数（表示杂志中字符不足）立即返回，避免不必要的完整遍历。
4. **数据结构选择**：使用固定大小的数组（仅限小写字母）或哈希表（通用解法）来存储字符计数。

### 代码1：数组解法（针对小写字母）
```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        # 初始化26个字母的计数数组
        record = [0] * 26
        
        # 统计杂志中各字符出现次数
        for c in magazine:
            record[ord(c) - ord('a')] += 1
        
        # 检查赎金信中的字符
        for c in ransomNote:
            # 计算字符索引并减少计数
            idx = ord(c) - ord('a')
            record[idx] -= 1
            # 若计数为负说明杂志中字符不足
            if record[idx] < 0:
                return False
        
        return True
```
### 代码2：哈希表解法（通用）
```Python

class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        # 初始化哈希表
        record = {}
        
        # 统计杂志中各字符出现次数
        for c in magazine:
            record[c] = record.get(c, 0) + 1
        
        # 检查赎金信中的字符
        for c in ransomNote:
            # 减少字符计数（若不存在则初始化为0后减1）
            record[c] = record.get(c, 0) - 1
            # 若计数为负说明杂志中字符不足
            if record[c] < 0:
                return False
        
        return True
```

### 复杂度分析
- 数组解法：
    - 时间复杂度：O(m + n) 其中 m 是 magazine 长度，n 是 ransomNote 长度。需要遍历两个字符串各一次。
    - 空间复杂度：O(1) 使用固定大小(26)的计数数组，与输入规模无关。
- 哈希表解法：
    - 时间复杂度：O(m + n) 同样需要遍历两个字符串各一次。
    - 空间复杂度：O(k) k 是字符集大小，最坏情况（包含所有不同字符）为 O(min(m, ∣Σ∣))，其中 ∣Σ∣ 是字符集大小。

- 两种解法时间复杂度相同，数组解法在空间效率上更优（O(1)），但仅适用于小写字母；哈希表解法通用性更强。



## [15. 三数之和](https://leetcode.cn/problems/3sum/)
### 第一想法
尝试用哈希表存储两数之和，但去重极其困难。想到排序但不确定具体实现。
### 学习后思路

1. **排序预处理**：
   - 对数组进行排序，使相同元素相邻，便于后续双指针操作和去重处理
   - 排序后，当固定元素大于0时，可直接终止搜索（右侧元素均大于0，不可能和为0）

2. **固定元素遍历**：
   - 遍历数组元素作为三元组的第一个元素（索引范围 0 到 n-3）
   - 跳过重复的固定元素：`if i > 0 and nums[i] == nums[i-1]: continue`

3. **双指针搜索**：
   - **初始化**：左指针 `left = i+1`，右指针 `right = n-1`
   - **指针移动**：
     - 和 > 0：右指针左移（`right--`）
     - 和 < 0：左指针右移（`left++`）
     - 和 = 0：记录解，并跳过左右重复元素

4. **去重优化**：
   - 在记录有效三元组后，同时跳过左侧和右侧的重复元素
   - 保证每个有效三元组只被记录一次

### 代码
```Python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        nums.sort() # 对数组进行排序，为双指针法和去重做准备
        result = []
        
        for i in range(0, n-2):

            # 如果当前元素大于0，由于数组已排序，后续元素都大于0，不可能组成和为0的三元组
            if nums[i] > 0:
                break

            # 跳过重复的固定元素（避免产生重复三元组）
            if i > 0 and nums[i] == nums[i-1]:
                continue 
            
            # 初始化左右指针：左指针指向固定元素的下一个位置，右指针指向数组末尾
            left = i + 1
            right = n - 1 
            
            # 双指针遍历剩余区间
            while left  < right:
                total = nums[i] + nums[left] + nums[right]
                # 和太大 右指针左移
                if total > 0: 
                    right -= 1
                # 和太小 左指针右移
                elif total < 0:
                    left += 1
                # 找到有效三元组
                else: 
                    result.append([nums[i], nums[left], nums[right]])
                    # 跳过左侧重复元素
                    while left  < right and nums[left] == nums[left + 1]:
                        left += 1
                    # 跳过右侧重复元素
                    while left  < right and nums[right] == nums[right - 1]:
                        right -= 1
                    # 同时移动左右指针寻找新的组合
                    left +=1
                    right -=1
        return result
```

### 复杂度分析
- 时间复杂度：O(n2) 
    - 排序：O(nlogn) 
    - 双指针搜索：外层循环O(n)，内层双指针O(n)，
    - 总时间复杂度：O(nlogn)+O(n2)=O(n2)
- 空间复杂度：O(1)（不考虑结果存储）
    - 排序使用O(logn) 栈空间（递归排序）
    - 双指针使用常数额外空间

## [18. 四数之和](https://leetcode.cn/problems/4sum/)


### 思路（第一想法直接在上一题的基础上多一重循环，竟然就接出来了，不过剪枝操作确实没想到）
1. **排序预处理**：
   - 对数组排序，使相同元素相邻，便于双指针操作和去重
   - 排序是后续所有优化（剪枝、去重）的基础

2. **双层循环+双指针**：
   - 外层循环遍历第一个数（索引范围 0 到 n-4）
   - 内层循环遍历第二个数（索引范围 i+1 到 n-3）
   - 双指针寻找剩余两个数（左指针从 j+1 开始，右指针从 n-1 开始）

3. **多级去重处理**：
   - 外层循环去重：`i > 0 and nums[i] == nums[i-1]`
   - 内层循环去重：`j > i+1 and nums[j] == nums[j-1]`
   - 双指针去重：找到解后跳过左右重复元素





### 复杂度分析
- **时间复杂度**：$O(n^3)$
  - 排序：$O(n \log n)$
  - 双层循环+双指针：外层 $O(n)$，内层 $O(n)$，双指针 $O(n)$ → $O(n^3)$
- **空间复杂度**：$O(1)$（不考虑结果存储）
  - 排序使用 $O(\log n)$ 栈空间
  - 双指针使用常数额外空间

### 代码
```Python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        n = len(nums)
        nums.sort() # 对数组进行排序，为双指针法和去重做准备
        result = []
        
        for i in range(0, n-3):
                        
            # 跳过重复的固定元素（避免产生重复元组）
            if i > 0 and nums[i] == nums[i-1]:
                continue 

            for j in range(i+1, n-2):
                if j > i+1 and nums[j] == nums[j-1]:
                    continue 
                    
                
                # 初始化左右指针：左指针指向固定元素的下一个位置，右指针指向数组末尾
                left = j + 1
                right = n - 1 
                
                # 双指针遍历剩余区间
                while left  < right:
                    total = nums[i] + nums[j] + nums[left] + nums[right]
                    
                    # 和太大 右指针左移
                    if total > target: 
                        right -= 1
                    
                    # 和太小 左指针右移
                    elif total < target:
                        left += 1
                    
                    # 找到有效四元组
                    else: 
                        result.append([nums[i], nums[j],nums[left], nums[right]])
                        # 跳过左侧重复元素
                        while left  < right and nums[left] == nums[left + 1]:
                            left += 1
                        # 跳过右侧重复元素
                        while left  < right and nums[right] == nums[right - 1]:
                            right -= 1
                        # 同时移动左右指针寻找新的组合
                        left +=1
                        right -=1
        return result
    
```
### 进一步优化空间
**多级剪枝优化**：
   - **外层循环剪枝**：
     - 当前最小和 > target：`nums[i]+nums[i+1]+nums[i+2]+nums[i+3] > target`
     - 当前最大和 < target：`nums[i]+nums[n-3]+nums[n-2]+nums[n-1] < target`
   - **内层循环剪枝**：
     - 固定 i 后最小和 > target：`nums[i]+nums[j]+nums[j+1]+nums[j+2] > target`
     - 固定 i 后最大和 < target：`nums[i]+nums[j]+nums[n-2]+nums[n-1] < target`


## 今日总结
通过分组哈希法将四数组问题降维处理（四数相加II），用字符频次统计与实时检测机制高效处理资源匹配问题（赎金信），运用排序预处理+双指针搜索解决有序数组的三数/四数之和。