## 704. 二分查找 
题目链接：https://leetcode.cn/problems/binary-search/

```Python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums)-1
        while left <= right:
            middle = left + (right - left) // 2
            if nums[middle] > target:
                right = middle - 1
            elif nums[middle] < target:
                left = middle + 1

            else:
                return middle
        return -1
```


 
## 27. 移除元素
题目链接：https://leetcode.cn/problems/remove-element/ 

```Python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i, k = 0, len(nums)
        while i < k:
            if nums[i] == val: 
                for j in range(i+1, k):
                    nums[j-1] = nums[j]
                
                k-=1
                i-=1
            i+=1
        return k
```


 
## 977.有序数组的平方 
题目链接：https://leetcode.cn/problems/squares-of-a-sorted-array/


```Python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        for i in range(len(nums)):
            nums[i] *= nums[i]
        nums.sort()
        return nums
```
