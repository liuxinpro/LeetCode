Day2_209长度最小的子数组_59螺旋矩阵II

## 209. 长度最小的子数组

题目链接：https://leetcode.cn/problems/minimum-size-subarray-sum/


### 我的理解
- 滑动窗口通过单次线性遍历和指针单向移动，避免了嵌套循环导致的平方复杂度。
- 每次窗口调整（移动左指针）均摊到整体遍历中，确保时间复杂度为线性。

### 我的代码（滑动窗口法）

```Python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        """使用滑动窗口思想"""
        left = 0
        right = 0

        min_len = float('inf')
        current_sum = 0 # 当前累加值

        while right < len(nums):
            # 不断移动右端点 搜索满足条件的数组

            current_sum += nums[right]

            while current_sum >= target:
                # 不断移动左端点 优化最小长度
                min_len = min(min_len, right-left+1)
                current_sum -= nums[left] 
                left += 1
            
            right +=1
        
        # 如果没找到满足条件的
        if min_len == float('inf'):
            min_len = 0
        
        return min_len
```





## 59.螺旋矩阵II


题目链接：https://leetcode.cn/problems/spiral-matrix-ii/

```Python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        
        if n <= 0:
            return []

        matrix = [[0] *n for _ in range(n)]

        top, bottom, left, right = 0, n-1, 0, n-1
        num = 1


        while top <= bottom and left <= right:
            
            # 从左到右填充上边界
            for i in range(left, right+1):
                matrix[top][i] = num
                num += 1
            top += 1

            
            # 从上到下填充右边界
            for i in range(top, bottom+1):
                matrix[i][right] = num
                num += 1
            right -= 1

            # 从右到左填充下边界
            for i in range(right, left-1, -1):
                matrix[bottom][i] = num
                num  += 1
            bottom -= 1
            
            # 从下到上填充左边界
            for i in range( bottom,top-1, -1):
                matrix[i][left] = num
                num += 1
            
            left += 1
        return matrix

```


