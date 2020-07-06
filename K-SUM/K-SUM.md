**167 Two Sum II - Input array is sorted.**

**Python**

```
# dictionary
class Solution:
    def twoSum(self, numbers, target):
        dic = {}
        ans = []
        for i in range(len(numbers)):
            if target - numbers[i] not in dic:
                dic[numbers[i]] = i
            else:
                return [dic[target-numbers[i]]+1,i+1]

# two pointers
class Solution:
    def twoSum(self, numbers, target):
        left = 0
        right = len(numbers)-1
        while left < right:
            if numbers[left] + numbers[right] == target:
                return [left+1, right+1]
            elif numbers[left] + numbers[right] < target:
                left += 1
            else:
                right -= 1

# binary search
class Solution:
    def twoSum(self, numbers target):
        for i in range(len(numbers)):
            cur = target - numbers[i]
            left = i+1
            right = len(numbers)-1
            while left <= right:
                mid = left + (right-left)//2
                if numbers[mid] == cur:
                    return [i+1,mid+1]
                elif numbers[mid] < cur:
                    left = mid+1
                else:
                    right = mid-1

```

**15. 3Sum**

**Python**
```
class Solution:
    def threeSum(self, nums):
        nums = sorted(nums)
        ans = []    
        for i in range(len(nums)-2):
            target = -1*(nums[i])
            if i-1 >= 0 and nums[i] == nums[i-1]:
                continue
            left = i+1
            right = len(nums)-1

            while left < right:
                if nums[left] + nums[right] == target:
                    ans.append([nums[i],nums[left],nums[right]])
                    left += 1
                    while nums[left] == nums[left-1] and left < right:
                        left += 1
                    right -= 1
                    while nums[right] == nums[right+1] and left < right:
                        right -= 1                 
                elif nums[left] + nums[right] < target:
                    left += 1        
                else:
                    right -= 1
        return ans
 ```

 **16. 3Sum Closest**

 **Python**
 ```
 class Solution:
    def threeSumClosest(self, nums, target):
        ans = float('Inf')
        nums = sorted(nums)
        for i in range(len(nums)-2):
            left = i+1
            right = len(nums)-1
            while left < right:
                cursum = nums[i] + nums[left] + nums[right]
                if cursum == target:
                    return target
                elif cursum < target:
                    left += 1
                else:
                    right -= 1
                if abs(cursum-target) < abs(ans-target):
                    ans = cursum

        return ans
```

**259. 3Sum Smaller**

**Python**
```
class Solution:
    def threeSumSmaller(self, nums, target):
        cnt = 0
        nums = sorted(nums)
        for i in range(len(nums)-2):
            left = i+1
            right = len(nums)-1
            while left < right:
                if nums[left] + nums[right] + nums[i] < target:
                    cnt = cnt + right - left
                    left += 1
                else:
                    right -= 1

        return cnt
```
