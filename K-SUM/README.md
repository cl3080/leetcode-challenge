K-SUM Problem

167 Two Sum II - Input array is sorted [Easy]

Code:

Python

'''
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

'''
