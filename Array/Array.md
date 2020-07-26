### Array

**53 Maximum Subarray**

**Python**

Solution 1: Divide Conquer
```
class Solution:
    def maxSubArray(self, nums):
        return self.helper(nums,0,len(nums)-1)

    def cross_sum(self,nums,left,right,p):
        if left == right:
            return nums[left]

        left_subsum = float('-inf')
        curr_sum = 0
        for i in range(p,left-1,-1):
            curr_sum += nums[i]
            left_subsum = max(left_subsum,curr_sum)

        right_subsum = float('-inf')
        curr_sum = 0
        for i in range(p+1,right+1):
            curr_sum += nums[i]
            right_subsum = max(right_subsum,curr_sum)

        return left_subsum + right_subsum

    def helper(self,nums,left,right):
        if left == right:
            return nums[left]

        p = (left + right)//2

        left_sum = self.helper(nums,left,p)
        right_sum = self.helper(nums,p+1,right)
        cross_sum = self.cross_sum(nums,left,right,p)

        return max(left_sum,right_sum,cross_sum)
```

Time complexity: O(nlogn)   
Space complexity: O(logn)

Solution 2: Greedy
```
def maxSubArray(self, nums):
    n = len(nums)
    curr_sum = max_sum = nums[0]

    for i in range(1,n):
        curr_sum = max(nums[i],curr_sum + nums[i])
        max_sum = max(max_sum,curr_sum)

    return max_sum
 ```
Time complexity: O(n)  
Space complexity: O(1)  

Solution 3: Dynamic programming
```
class Solution:
    def maxSubArray(self, nums):
        n = len(nums)
        dp = [0 for i in range(len(nums))]
        dp[0]  = nums[0]
        for i in range(1,len(nums)):
            dp[i] = max(dp[i-1],0) + nums[i]            
        return max(dp)
```

**78 Subsets**

**Python**

Solution 1: Cascading
```
class Solution:
    def subsets(self, nums):
        n = len(nums)
        output = [[]]  
        for num in nums:
            output += [curr + [num] for curr in output]       
        return output
```

Solution 2: dfs + backtracking
```
class Solution:
    def subsets(self, nums):  
        ans = []
        def dfs(n,s,curr):
            if len(curr) == n:
                ans.append(curr.copy())
                return

            for i in range(s,len(nums)):
                curr.append(nums[i])
                dfs(n,i+1,curr)
                curr.pop()

        for i in range(len(nums)+1):
            dfs(i,0,[])

        return ans
```

Solution 3: Lexicographic (binary sorted) subsets
```
class Solution:
    def subsets(self, nums):  
        n = len(nums)
        output = []
        for i in range(2**n,2**(n+1)):
            bitmask = bin(i)[3:]
            output.append([nums[j] for j in range(n) if bitmask[j] == '1'])
        return output
```

**90 Subsets II**

**Python**

```
class Solution:
    def subsetsWithDup(self, nums):   
        nums.sort()
        res = []
        self.dfs(nums,0,[],res)
        return res

    def dfs(self,nums,start,subset,res):
        if subset not in res:
            res.append(subset)
        for i in range(start,len(nums)):
            if i > start and nums[i] == nums[i-1]:
                continue
            self.dfs(nums,i+1,subset+[nums[i]],res)
```

**152 Maximum Product Subarray**

**Python**

Solution: dynamic programming

```
class Solution:
    def maxProduct(self, nums):
        if len(nums) == 0:
            return 0

        max_so_far = nums[0]
        min_so_far = nums[0]
        result = max_so_far

        for i in range(1,len(nums)):
            curr = nums[i]
            temp_max = max(curr,curr*max_so_far, curr*min_so_far)
            min_so_far = min(curr,max_so_far*curr, curr*min_so_far)
            max_so_far = temp_max
            result = max(max_so_far, result)

        return result
```

**228 Summary Ranges**

**Python**

Solution: two pointers
```
class Solution:
    def summaryRanges(self, nums):
        ans = []
        i,j = 0,0
        while i < len(nums):
            j = i

            while i + 1 < len(nums) and nums[i+1] == nums[i] +1:
                i += 1     
            if i == j:
                curr = str(nums[i])
                ans.append(curr)
                i += 1
            else:
                curr = str(nums[j]) + "->" + str(nums[i])
                ans.append(curr)
                i += 1

        return ans
```

**163 Missing Ranges***

**Python**
```
class Solution:
    def findMissingRanges(self, nums, lower, upper):
        nums = [lower-1] + nums + [upper+1]
        ans = []
        for i in range(1,len(nums)):
            diff = nums[i] - nums[i-1]
            if diff == 2:
                ans.append(str(nums[i-1]+1))
            elif diff > 2:
                start = nums[i-1]+1
                end = nums[i]-1
                ans.append(str(start) + '->' + str(end))      
        return ans
```

**325 Maximum Size Subarray Sum Equals K**

**Python**
```
class Solution:
    def maxSubArrayLen(self, nums, k):
        dic = {0:-1}
        maxlen = 0
        summ = 0

        for i in range(len(nums)):
            summ = summ + nums[i]
            if summ - k in dic and i-dic[summ-k] > maxlen:
                maxlen = i - dic[summ-k]
            if summ not in dic:
                dic[summ] = i

        return maxlen
```

**209 Minimum Size Subarray Sum **

**Python**

Solution 1: Brute Force
```
class Solution:
    def minSubArrayLen(self, s, nums):
        length  = len(nums)
        if length == 0:
            return 0
        ans = float('inf')
        summ = [length for i in range(len(nums))]
        summ[0] = nums[0]
        for i in range(1,len(nums)):
            summ[i] = summ[i-1] + nums[i]

        for i in range(len(nums)):
            for j in range(i,len(nums)):
                cur_summ = summ[j] - summ[i] + nums[i]
                if cur_summ >= s:
                    ans = min(ans,j-i+1)
                    break

        if ans == float('inf'):
            return 0
        else:
            return ans
```

Solution 2: Two pointers
```
class Solution:
    def minSubArrayLen(self, s, nums):
        left = 0
        ans = float('inf')  
        summ = 0

        for i in range(len(nums)):
            summ = summ + nums[i]
            while summ >= s:
                ans = min(i-left+1,ans)
                summ = summ - nums[left]
                left += 1

        if ans == float('inf'): return 0
        return ans
```

Solution 3: Binary Search
```
class Solution:
    def minSubArrayLen(self, s nums):
        size = len(nums)
        res = float('inf')
        summ = [0 for i in range(size+1)]
        for i in range(1,size+1):
            summ[i] = summ[i-1] + nums[i-1]
        for i in range(size):
            left = i+1
            right = size
            t = summ[i] + s
            while left <= right:
                mid = left + (right - left)//2
                if summ[mid] < t:
                    left = mid + 1
                else:
                    right = mid - 1

            if left == size+1:
                break

            res = min(res,left-i)        

        if res == float('inf'):
            return 0
        else:
            return res
```

**238 Product of Array Except Self**
```
class Solution:
    def productExceptSelf(self, nums):
        left = [1 for i in range(len(nums))]
        right = [1 for i in range(len(nums))]

        for i in range(1,len(nums)):
            left[i] = left[i-1]*nums[i-1]

        for i in range(len(nums)-2,-1,-1):
            right[i] = right[i+1]*nums[i+1]

        ans = []
        for i in range(len(nums)):
            curr = right[i]*left[i]
            ans.append(curr)

        return ans
```


Solution 2: O(1) Space
```
class Solution:
    def productExceptSelf(self, nums):
        ans = [1 for i in range(len(nums))]

        for i in range(1,len(nums)):
            ans[i] = ans[i-1]*nums[i-1]

        R = 1
        for i in range(len(nums)-1,-1,-1):
            ans[i] = R*ans[i]
            R = nums[i]*R

        return ans
```

**239 Sliding Window Maximum**

Solution 1: Priority Queue
```
class Solution:
    def maxSlidingWindow(self, nums, k):
        from collections import deque
        n = len(nums)
        if n*k == 0: return []
        if k == 1: return nums

        def clean_deque(i):
            if deq and deq[0] == i-k:
                deq.popleft()

            while deq and nums[i] > nums[deq[-1]]:
                deq.pop()

            deq.append(i)

        deq = deque()
        max_idx = 0
        ans = []
        for i in range(k):
            clean_deque(i)
            if nums[i] > nums[max_idx]:
                max_idx = i
        ans.append(nums[max_idx])

        for i in range(k,n):
            clean_deque(i)
            ans.append(nums[deq[0]])

        return ans
```

Solution 2: Dynamics programming
```
class Solution:
    def maxSlidingWindow(self, nums, k):
        n = len(nums)
        if n*k == 0: return []
        if k == 1: return nums

        left = [0]*n
        left[0] = nums[0]
        right = [0]*n
        right[-1] = nums[-1]
        for i in range(1,n):
            if i%k == 0:
                left[i] = nums[i]
            else:
                left[i] = max(left[i-1],nums[i])

            j = n-i-1
            if (j+1)%k == 0:
                right[j] = nums[j]
            else:
                right[j] = max(right[j+1],nums[j])

        output = []
        for i in range(n-k+1):
            output.append(max(left[i+k-1],right[i]))

        return output
```

**128. Longest Consecutive Sequence**

```
class Solution:
    def longestConsecutive(self, nums):
        longest_streak = 0
        num_set = set(nums)

        for num in num_set:
            if num - 1 not in num_set:
                current_num = num
                current_streak = 1

                while current_num + 1 in num_set:
                    current_num += 1
                    current_streak += 1
                longest_streak = max(longest_streak,current_streak)

        return longest_streak
```
