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
