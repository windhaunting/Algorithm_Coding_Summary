# Backtracking

Backtracking is a technique that building candidates to the solutions incrementally, one piece at a time, removing those solutions that fail to satisfy the constraints of the problem at any point in time till reaching any level of the search tree. It is different from exhaustive search, which does not generate all possible solutions first and checks later.  Backtracking tries all	possible	paths and then abandons them if they are not suitable.	



### Backtracking Pseduo Code

```
Backtrack(decisions):
 – if there are no more decisions to make:
     • if our current solution is valid, return true 
     • else, return false
 – else, let's handle one decision ourselves, and the rest by recursion.
  for each available valid choice C for this decision: 
  • Choose C by modifying parameters. 
  • Explore the remaining decisions that could follow C. If any of them find a solution, return true 
  • Un-choose C by returning parameters to original state (if necessary). 
  – If no solutions were found, return false

```

### Backtracking Example: 46. Permutations
Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order. Please refer [<span style="color:blue;"> here </span>](https://leetcode.com/problems/permutations/) for more detail.


```
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:    
        # one way to do recursion
        ans = []
        def helper(nums, indx, curr):
            if len(nums) == indx:
                ans.append(curr)
                return
            
            for i in range(0, len(nums)):
                
                if nums[i] not in curr:
                    helper(nums, indx+1, curr + [nums[i]])
                    
        curr = []
        
        helper(nums, 0, curr)
        return ans
```


##### Another way to do this:
```
def permuationArray(nums):
    
    def dfsPermutation(nums, l, r):
        
        if l == r:
            self.res_list.append(nums)
            print ("ssss: ", self.res_list)
        else:
            for i in range(l, r+1):
                nums[l], nums[i] = nums[i], nums[l]          # swap operation in the python
                #print ("before l, r: ", i, l, r, res_list)
                self.dfsPermutation(nums, l+1, r)

                #print ("l, r: ", i, l, r, res_list)
                nums[l], nums[i] = nums[i], nums[l]  
                
    dfsPermutation(nums, 0, len(nums)-1)

```

### Backtracking Example: 77. Combinations
Given two integers n and k, return all possible combinations of k numbers out of the range [1, n]. Please refer [<span style="color:blue;"> here </span>](https://leetcode.com/problems/combinations/) for more detail. 

```
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        # idea not use backtracking.  if n = 4, k =2 , we select 1,2,3,4 each tree branch, then the next brach select the rest 3 number  like the permutation of An(k) = time complexity
    
            
        def helper(n, k, pos, cur_lst, res_lst):
            
            if len(cur_lst) == k:
                res_lst.append(cur_lst)
                return            
            
            for i in range(pos, n+1):
                helper(n, k, i+1, cur_lst + [i], res_lst)
            
        res_lst = []
        pos = 1
        cur_lst = []
        helper(n, k, pos, cur_lst, res_lst)
        #print ("res_lsttt", res_lst)
        
        return res_lst

```

### More Backtracking Examples:

[<span style="color:blue;"> Leetcode 78. Subsets </span>](https://leetcode.com/problems/subsets/) for more detail.

[<span style="color:blue;"> Leetcode 51. N-Queens </span>](https://leetcode.com/problems/n-queens/) for more detail.

