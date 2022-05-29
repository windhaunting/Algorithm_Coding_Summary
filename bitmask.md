
# Bitmask

Bitmask is a technique using bit operation or bit representation to achieve some purpose to save time and space complexity. 
Each bit could represent an operation or another integer.

### Example bitmask: 464. Can I Win
The detailed decription is here: "https://leetcode.com/problems/can-i-win/"


```
class Solution(object):
    def canIWin(self, maxChoosableInteger, desiredTotal):
        """
        :type maxChoosableInteger: int
        :type desiredTotal: int
        :rtype: bool
        """
        winMap = {}
        def canIWinHelper(total, visited):
            if visited in winMap:
                return winMap[visited]
            for i in range(1, maxChoosableInteger+1):
                mask = (1 << i)
                if (mask & visited) == 0 and (i >= total or not canIWinHelper(total - i, mask | visited)):
                    winMap[visited] = True
                    return True
            winMap[visited] = False
            return False
        
        # corner case
        if maxChoosableInteger > desiredTotal:
            return True
        if (1+maxChoosableInteger)*maxChoosableInteger/2 < desiredTotal:      #sum
            return False
        
        return canIWinHelper(desiredTotal, 0)

```


### Example bitmask: 698. Partition to K Equal Sum Subsets
The detailed decription is here: "https://leetcode.com/problems/partition-to-k-equal-sum-subsets/"


```
class Solution:
    def canPartitionKSubsets(self, nums: List[int], k: int) -> bool:
        
        
        # search sum/k = target,     for each group rest,
        # also ele in nums used or not, using bitmask for the number
        
        #nums = [4, 3, 2, 3, 5, 2, 1], k = 4
        s = sum(nums)
        if s % k != 0:
            return False
        
        target = s // k
        # sort 
        nums.sort(reverse = True)
        if nums[-1] > target:  #  max(nums) > target: 
            return False
        
        
        def canPartitionHelper(nums, mask, cur_sum, target, dp): 
            # bitmask, cur_sum need to be found in the nums
            if mask in dp:
                return dp[mask]
            if cur_sum == 0:
                return True
            
            # resest in one group, if one group satisfied, the other group must start from target again, so use                     #(cur_sum -1) % target + 1.  like target = 4, current_sum = 16 now, the next rest should be 4, not 0

            res_in_one_group = (cur_sum-1) % target + 1   
            
            for i in range(len(nums)):
                # if not used for current i
                if (mask & (1 << i)) == 0 and nums[i] <= res_in_one_group:
                    if canPartitionHelper(nums, mask | (1 << i), cur_sum - nums[i], target, dp):
                        dp[mask] = True
                        return True
            
            return dp[mask]
                    
        dp = collections.defaultdict(bool)
        return canPartitionHelper(nums, 0, s, target, dp)

```