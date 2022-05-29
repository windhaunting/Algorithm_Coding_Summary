# Prefix sum

 Prefix sum is a good technique to pre-calculate the sum of subarray of an array. It could save the time complexity in the cost of space complexity. It could be extended to prefix-multiplication and others.

 ### Prefix sum example: 209. Minimum Size Subarray Sum
 Given an array of positive integers nums and a positive integer target, return the minimal length of a contiguous subarray [numsl, numsl+1, ..., numsr-1, numsr] of which the sum is greater than or equal to target. If there is no such subarray, return 0 instead.
 The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/minimum-size-subarray-sum/)

```
        if not nums:
            return 0
        #get sumarr arrary as the prefix sum array
        minLen = len(nums) + 1
        sumarr = [0 for i in range(0, len(nums)+1)]
        
        sumarr[1] = nums[0]
        for i in range(1, len(nums)+1):
            sumarr[i] = nums[i-1] + sumarr[i-1]
        print (' sumarr ', sumarr) 
        
        for i in range(0, len(nums)):
            rightEnd = self.searchBinaryRight(sumarr, i, len(nums), s + sumarr[i])
            if rightEnd == len(sumarr):
                break
            if rightEnd <= len(nums):
                minLen = min(minLen, rightEnd - i)
        return minLen if minLen != len(nums) + 1 else 0
        
    def searchBinaryRight(self, sumarr, start, end, target):
        while start  <= end:
            mid = (start + end)/2
            if sumarr[mid] >= target:
                end = mid - 1
            else:
                start = mid + 1
        return start

```

 ### Prefix sum/multi example: 238. Product of Array Except Self
Given an integer array nums, return an array answer such that answer[i] is equal to the product of all the elements of nums except nums[i]. The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/product-of-array-except-self/)

```
        # origin 1  2   3  4
        # left  1   1   2  6
        # right 24  12  4  1 
        #return 24  12  8  6
        
        if len(nums) == 0 or nums is None:
            return []
        
        s = [1] * len(nums)
        for i in range(1, len(nums)):
            s[i] = s[i-1] * nums[i-1]              # prefix multi arrary
            
        #print ("s: ", s)
        p = 1
        for i in range(len(nums)-1, -1, -1):
            s[i] *= p
            p *= nums[i]
            #print ("p : ", p, i, s[i])
            
        return s
```