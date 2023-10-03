
# Two pointers

It uses two pointers (left and right pointers) to dynamically select sub intervals in a sequence.
It commonly used as looping, find answers in a subarray based on two pointer intervals


### 167. Two Sum II - Input Array Is Sorted

Given a 1-indexed array of integers numbers that is already sorted in non-decreasing order, find the indices of two numbers such that the two numbers add up to a specific target number.  The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

```
     def twoSum(self, numbers, target):
        """
        :type numbers: List[int]
        :type target: int
        :rtype: List[int]
        """
        # Use l and r pointers

        l = 0
        r = len(numbers)-1
        
        if r <= 1:
            return []
        while (l < r):
            s = numbers[l] + numbers[r]
            if s == target:
                return [l+1, r+1]
            elif s > target:
                r = self.binarySearch(numbers, l, r-1,  target - numbers[l])   # here use binary search as well
            else:
                l = self.binarySearch(numbers, l + 1, r, target - numbers[r])
        return []
    def binarySearch(self, numbers, start, end, tgt):
        while (start < end):
            mid = (start + end)/2
            if numbers[mid] == tgt:
                return mid
            elif numbers[mid] < tgt:
                start = mid + 1
            else:
                end = mid - 1
        return start

```

### 11. Container With Most Water

You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]). Find two lines that together with the x-axis form a container, such that the container contains the most water. Return the maximum amount of water a container can store. The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/container-with-most-water/)

```
class Solution(object):
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        
#  use two pointer l from 0 to right, r from the most rightward to 0

        n = len(height)
        
        l = 0
        r = n - 1
        ans_area = 0
        while(l < r):
            
            lt = height[l]
            rt = height[r]
            ans_area = max(ans_area, (r-l) * min(lt, rt))
            
            if lt <= rt:
                l += 1
            else:
                r -= 1
    
        return ans_area

```


### 42. Trapping Rain Water

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining. The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/trapping-rain-water/)



```
class Solution(object):
    def trap(self, height):
        """
        :type height: List[int]
        :rtype: int
        """

      # idea 1 use two loops SUCCESS
        res = 0
        max_ht_dict = collections.defaultdict(int)
        mx = 0

        n =  len(height)
        for i in range(0, n):
            max_ht_dict[i] = mx            # get each index's height's bigger height at left most side's height
            mx = max(mx, height[i])

        mx = 0
        for i in range(n-1, -1, -1):
            max_ht_dict[i] = min(max_ht_dict[i], mx)   # find the right most's bigger height and compared left most bigger height, and then get the minimum value
            mx = max(mx, height[i])              

            if max_ht_dict[i] > height[i]:
                res += max_ht_dict[i] - height[i]

        return res
        '''
        
        # use one iteration
        l = 0
        r = len(height)-1
        res = 0
        while (l < r):
            
            min_val = min(height[l], height[r])
            
            if min_val == height[l]:
                
                l += 1
                while(l < r and height[l] < min_val):
                    res += min_val - height[l]
                    l += 1
            else:
                r -= 1
                while(l < r and height[r] < min_val):
                    res += min_val - height[r]
                    r -= 1
        return res

```
