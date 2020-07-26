### Rotate

**48 Rotate Image**

**Python**
```
class Solution:
    def rotate(self, matrix):
        n = len(matrix[0])        
        # transpose matrix
        for i in range(n):
            for j in range(i, n):
                matrix[j][i], matrix[i][j] = matrix[i][j], matrix[j][i]

        # reverse each row
        for i in range(n):
            matrix[i].reverse()
```

**33 Search in Rotated Sorted Array**

**Python**
```
class Solution:
    def search(self, nums, target):
        start = 0
        end = len(nums)-1

        while start <= end:
            mid = start + (end-start)//2
            if nums[mid] == target:
                return mid
            elif nums[mid] >= nums[start]:
                if target >= nums[start] and target < nums[mid]:
                    end = mid-1
                else:
                    start = mid+1
            else:
                if target <= nums[end] and target > nums[mid]:
                    start = mid+1
                else:
                    end = mid - 1

        return -1
```

**81 Search in Rotated Sorted Array II**

**Python**
```
class Solution:
    def search(self, nums, target):
        n = len(nums)
        left, right = 0,n-1
        while (left <= right):
            mid = (left + right)//2
            if nums[mid] == target:
                return True
            elif nums[mid] < nums[right]:
                if target > nums[mid] and nums[right] >= target:
                    left = mid + 1
                else:
                    right = mid -1
            elif nums[mid] > nums[right]:
                if target >= nums[left] and nums[mid] > target:
                    right = mid-1
                else:
                    left = mid+1
            else:
                right -= 1

        return False
```
