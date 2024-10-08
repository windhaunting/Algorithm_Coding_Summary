
# Greey algorithm
Greedy algorithm is an algorithm paradigm that follows the problem-solving heuristic of making the locally optimal choice at each stage. It chooses locally optimal at each stage greedily that leads to global solution. It is  memory efficient that never looking back or revising previous choices.



### Greedy algorithm example: Fractional Knapsack
 Given the weights and values of n items, we need to put these items in a knapsack of capacity W to get the maximum total value in the knapsack. we can break items for maximizing the total value of knapsack.


```

class ItemValue:
 
    """Item Value DataClass"""
 
    def __init__(self, wt, val, ind):
        self.wt = wt
        self.val = val
        self.ind = ind
        self.cost = val // wt
 
    def __lt__(self, other):          # sort the cost
        return self.cost < other.cost
 
# Greedy Approach

class FractionalKnapSack:
 
    """Time Complexity O(n log n)"""
    @staticmethod
    def getMaxValue(wt, val, capacity):
        """function to get maximum value """
        iVal = []
        for i in range(len(wt)):
            iVal.append(ItemValue(wt[i], val[i], i))
 
        # sorting items by value
        iVal.sort(reverse=True)
 
        totalValue = 0
        for i in iVal:
            curWt = int(i.wt)
            curVal = int(i.val)
            if capacity - curWt >= 0:
                capacity -= curWt
                totalValue += curVal
            else:
                fraction = capacity / curWt
                totalValue += curVal * fraction
                capacity = int(capacity - (curWt * fraction))
                break
        return totalValue

```


### Greedy algorithm example LC 45. Jump Game II: 
Given an array of non-negative integers nums, you are initially positioned at the first index of the array. Each element in the array represents your maximum jump length at that position. Your goal is to reach the last index in the minimum number of jumps. The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/jump-game-ii/)


```
class Solution(object):
    def jump(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        
        '''
        # Greedy algorithm: 
        The basic thoughts underline is a greedy style. Every one more jump, you want to jump as far as possible.
        In Jump Game I, when you at position i, you care about what is the furthest position could be reached from i th position. but here in Jump Game II, instead you care about what would be the next furthest jump could be made when you could reach as far as ith position from last jump.
     So you iterate all positions could be reached from last jump till i th position to find it out.

        '''
        jumps = curEnd = curFarthest = 0
        for i in range(len(nums) - 1):
            curFarthest = max(curFarthest, i + nums[i])
            if i == curEnd:
                jumps += 1
                curEnd = curFarthest
        return jumps

```
### Greedy algorithm example 435. Non-overlapping Intervals
Given an array of intervals intervals where intervals[i] = [starti, endi], return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping. The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/non-overlapping-intervals/description/)

```
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        
        """
        use two pointers, greedy algorithm
        # sort them
        [1, 2], [1, 3], [3, 4],
         prev     curr
          compare current pointer index with the previous pointer;
          delete list with big smaller end (not really deleting needed, just update the previous pointer);
        => [1, 2], [3, 4] and continue
        """

        if not intervals or len(intervals) <= 1:
            return 0

        # sort by start first
        intervals.sort(key=lambda ele: (ele[0], ele[1]))

        prev = intervals[0]
        ans = 0
        for curr in intervals[1:]:
            # find overlapping
            max_start = max(prev[0], curr[0])
            min_end = min(prev[1], curr[1])
            if max_start < min_end:  # overlapping
                if curr[1] < prev[1]: # delete first one by updating the prev
                    prev = curr
                ans += 1
            else:
                prev = curr
        return ans
```
