

# Binary indexed tree  or Fenwick tree

Problem decription:
    
We have an array arr[0 . . . n-1]. We should be able to

1. Find the sum of first i elements.

2. Change value of a specified element of the array arr[i] = x where 0 <= i <= n-1.


# Solution:

Using Binary Indexed Tree, we can do both tasks in O(Logn) time.
The advantages of Binary Indexed Tree over Segment are, requires less space and very easy to implement

It takes O(nlogn) to create the tree,   and take o(logn) time or space to query or update.



```

class NumArray(object):       # NumArray is just subaray tree
    def __init__(self, nums):
        """
        initialize your data structure here.
        :type nums: List[int]
        """
        self.sum_array = [0] * (len(nums) + 1)         # 1 more for binary indexed tree's dummy node?  sum_array[0] is the dummy node
        self.nums = nums
        self.n = len(nums)
        # create binary indexed tree
        for i in range(len(nums)):
            self.add(i + 1,nums[i])
    
    
    def add(self,i,val):
        while i <= self.n:
            self.sum_array[i] += val
            i += self.lowbit(i)
    
    
    def lowbit(self,i):
        return i & -i          # parent node?     
    
    def sum_to_index(self,i):
        res = 0
        while i >0:
            res += self.sum_array[i]
            i -= self.lowbit(i)
        return res
    
    def update(self, i, val):     # update nums[i] to val
        """
        :type i: int
        :type val: int
        :rtype: int
        """
        self.add(i + 1, val - self.nums[i])
        self.nums[i] = val
    
    def sumRange(self, i, j):
        """
        sum of elements nums[i..j], inclusive.
        :type i: int
        :type j: int
        :rtype: int
        """
        if not self.nums: return  0
        return self.sum_to_index(j+1) - self.sum_to_index(i)


```

## Reference:  
https://www.geeksforgeeks.org/binary-indexed-tree-or-fenwick-tree-2/
 
http://blog.csdn.net/u013974420/article/details/41942435
 
https://leetcode.com/problems/range-sum-query-mutable/discuss/75842/my-python-solution-using-fenwick-tree-208ms
 