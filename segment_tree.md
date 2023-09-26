# Segment Tree
 Segment tree is a tree data structure to store information about intervals, or segments. It allows answering a given point in a range over an array effectively, while still being flexible enough to allow modifying the array.



### One implementation of Segment Tree

 ``` 

# here is range sum segment tree, range minimum can also be used with segment tree
# find minimum range of a subarray in o(logn)

#1st use tree left right node to implementation segment tree
#2nd use array  i,  2*i + 1, 2*i + 2

# 1st method
class Node(object):
    def __init__ (self, start, end, ss = 0, left = None, right = None):
        self.start = start           # start index of a node
        self.end = end              # end index of a node
        self.ss = ss               # sum
        self.left = left            # left node
        self.right = right           # right node
        
class NumArray(object):
    def __init__ (self, nums):
        '''
        :type nums: list[int]
        '''
        self.root = None
        
    def createTree(self, nums, l, r):
        '''
        create segment tree
        '''
        
        #base case
        if l > r:
            return None
        
        # leaf node:
        if l == r:
            nd = Node(l, r)
            nd.ss = nums[l]
            return nd
        
        # otherwise internal node
        mid = l + (r-l)//2   #segment half
        
        #recursively create tree
        left = self.createTree(nums, l, mid)
        right = self.createTree(nums, mid+1, r)
        
        print("left.ss:", left.ss, right.ss, left, right)
        root = Node(l, r, left.ss + right.ss, left, right)
        return root
    
    
    def updateNode(self, index, val):
        '''
        update an array index value
        here use recursive way to update
        '''
        def updateValHelper(root, index, val):
            # base case, the leaf node encountered
            if root.start == root.end:
                root.ss = val
                return val
            
            mid = root.start + (root.end - root.start) // 2
            
            if index <= mid:       #go left
                updateValHelper(root.left, index, val)
            else:
                updateValHelper(root.right, index, val)
                
            #update parent sum
            root.ss = root.left.ss + root.right.ss
            return root.ss
        
        return updateValHelper(self.root, index, val)
            
    def rangeSumQuery(self, lInd, rInd):
        '''
        query content between left and right index
        '''
        def rangeSumHelper(root, lInd, rInd):
            
            #if rInd > root.end or lInd < root.start:
            #    return 0
            
            #inside
            if lInd == root.start and rInd == root.end:
                return root.ss
            
            mid = root.start + (root.end - root.start) // 2
              
            if rInd <= mid:
                return rangeSumHelper(root.left, lInd, rInd)
            
            elif lInd >= mid + 1:
                return rangeSumHelper(root.right, lInd, rInd)
        
            else:
                #overlapping with both 
                return rangeSumHelper(root.left, lInd, mid) + rangeSumHelper(root.right, mid+1, rInd)

        return rangeSumHelper(self.root, lInd, rInd)
    


nums = [1, 2,3,4,5]
NumArrayObj = NumArray(nums) 
NumArrayObj.root = NumArrayObj.createTree(nums, 0, len(nums)-1)

print ("range sum " , NumArrayObj.root.left.ss)
print ("range sum " , NumArrayObj.rangeSumQuery(2,4))
print ("range sum " , NumArrayObj.rangeSumQuery(1,1))

NumArrayObj.updateNode(2,10)
print ("range sum " , NumArrayObj.rangeSumQuery(2,4))

 ```


### Another implementation of Segment Tree


```
segment tree code 1: use an array to store tree


# 

class NumArray(object):

    def __init__(self, nums):
        """
        :type nums: List[int]
        """
        self.nums = nums
        self.sumLst = [0]*(len(nums)*3)    # use the array to represent the tree
        
        if not nums or len(nums) == 0:
            return
        self.createSegTree(nums)      # create tree
        
    def update(self, i, val):
        """
        :type i: int
        :type val: int
        :rtype: None
        """
        l = 0
        r = len(self.nums)-1
        self.updateSumLst(i, val, l, r, 0)
        
    def  updateSumLst(self, i, val, l, r, nd_ind):
    
        if l == r:
            self.nums[i] = val
            self.sumLst[nd_ind] = val
        else: 
            mid = l + (r-l)/2

            left_ind = 2*nd_ind + 1
            right_ind = 2*nd_ind +2
            if i >= l and i <= mid:  # go to left
                self.updateSumLst(i, val, l, mid, left_ind)
            else:
                self.updateSumLst(i, val, mid+1, r, right_ind)

            self.sumLst[nd_ind] = self.sumLst[left_ind] + self.sumLst[right_ind]
        
            
    def sumRange(self, i, j):
        """
        :type i: int
        :type j: int
        :rtype: int
        """
        l = 0
        r = len(self.nums)-1
        return self.sumRangeHelper(l, r, 0, i, j)
        
    def sumRangeHelper(self, l, r, nd_ind, i, j):
        # check i and j
        if r < i or j < l:
            return 0
        
        #if l == r:
        #    return self.sumLst[nd_ind]
        if i <= l and r <=j:              # find the range
            #print("ssse: ", l, r,nd_ind,self.sumLst[nd_ind])
            return self.sumLst[nd_ind]
        mid = l + (r-l)/2
        
        left_ind = 2*nd_ind + 1
        right_ind = 2*nd_ind +2
        
        sum_left = self.sumRangeHelper(l, mid, left_ind, i, j)
        sum_right = self.sumRangeHelper(mid+1, r, right_ind, i, j)
        return sum_left + sum_right
    
        
    def createSegTree(self, nums):
        l = 0
        r = len(nums)-1
        
        self.createSegTreeHelper(nums, l, r, 0, self.sumLst)
        #print("sumLst: ", self.sumLst)
        #return self.sumLst
    
    # build segment tree
    # https://www.youtube.com/watch?v=e_bK-dgPvfM
    def createSegTreeHelper(self, nums, l, r, nd_ind, sumLst):
        
        if l == r:
            sumLst[nd_ind] = nums[l]
        
        else:
            mid = l + (r-l)/2

            left_ind = 2*nd_ind + 1
            right_ind = 2*nd_ind + 2

            self.createSegTreeHelper(nums, l, mid, left_ind, sumLst)
            self.createSegTreeHelper(nums, mid+1, r, right_ind, sumLst)

            sumLst[nd_ind] = sumLst[left_ind] + sumLst[right_ind]

```

### More efficient implementation using two size of the input array 

Please ref: https://www.geeksforgeeks.org/segment-tree-efficient-implementation/

