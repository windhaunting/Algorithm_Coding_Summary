# Sampling algorithms

There are many types of sampling techniques:

Importance sampling;
Rejection sampling;
Inverse sampling;
Weighted sampling;
Reservoir sampling;
Bootstrap sampling

Here we introduce two important sampling in coding interview. Reversoir sampling and Rejection sampling.

## Reservoir sampling

 Reversoir sampling is a randomized algorithm for randomly choosing k samples (without replacement) from a list of n items in a single pass, where n is either a very large or unknown number. It is good for sampling an element or a sublist from a stream.

## Rejection sampling
 Rejection sampling is a sampling technique used to generate observations from a distribution.
 It is a monte carlo algorithm to sample data from a sophisticated (“difficult to sample from”) distribution with the help of a proxy distribution.


 ### Implementation of reservoir sampling

 ```

# An efficient Python3 program
# to randomly select k items
# from a stream of items
import random
# A utility function
# to print an array
def printArray(stream,n):
    for i in range(n):
        print(stream[i],end=" ");
    print();
 
# A function to randomly select
# k items from stream[0..n-1].
def selectKItems(stream, n, k):
        i=0;
        # index for elements
        # in stream[]
         
        # reservoir[] is the output
        # array. Initialize it with
        # first k elements from stream[]
        reservoir = [0]*k;
        for i in range(k):
            reservoir[i] = stream[i];
         
        # Iterate from the (k+1)th
        # element to nth element
        while(i < n):
            # Pick a random index
            # from 0 to i.
            j = random.randrange(i+1);
             
            # If the randomly picked
            # index is smaller than k,
            # then replace the element
            # present at the index
            # with new element from stream
            if(j < k):
                reservoir[j] = stream[i];
            i+=1;
         
        print("Following are k randomly selected items");
        printArray(reservoir, k);
     
# Driver Code
 
if __name__ == "__main__":
    stream = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12];
    n = len(stream);
    k = 5;
    selectKItems(stream, n, k);
 
# This code is contributed by mits


 ```


 ### Example of reservoir sampling: 398. Random Pick Index
The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/random-pick-index/)

 ```
import random

class Solution(object):

    def __init__(self, nums):
        """
        
        :type nums: List[int]
        :type numsSize: int
        """
        self.nums = nums

    def pick(self, target):
        """
        :type target: int
        :rtype: int
        """
        
        count, index = 0, 0
        for i, e in enumerate(self.nums):
            if e == target:
                count += 1
                rand = random.randint(1, count)
                if rand == count:
                    index = i

        return index

 ```


 ### Example of reservoir sampling: 398. Random Pick Index
The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/random-pick-index/)

 ```
import random

class Solution(object):

    def __init__(self, nums):
        """
        
        :type nums: List[int]
        :type numsSize: int
        """
        self.nums = nums

    def pick(self, target):
        """
        :type target: int
        :rtype: int
        """
        
        count, index = 0, 0
        for i, e in enumerate(self.nums):
            if e == target:
                count += 1
                rand = random.randint(1, count)
                if rand == count:
                    index = i

        return index

 ```


 ### Example of weighted sampling 528. Random Pick with Weight 
 The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/random-pick-with-weight/)

```

import random
class Solution:

    def __init__(self, w: List[int]):
        self.w = w
        
        if not self.w or len(self.w) == 0:
            return 0
        # prefix sum
        for i in range(1, len(self.w)):
            self.w[i] +=self.w[i-1]
            
    def pickIndex(self) -> int:
        #idea 1. copy the times of each index weight into a new array, in the new array, each value is picked randomly and equally, which has the same probability as the number of the original weight.
        # problem: too costly for the space.
        
        # idea 2 use prefix sum, accumulated sum of weight 
        # e.g.  [1,3,2]  => index [0,1,2], we get prefix sum[1,4,6]
        # then we pick number from 0 to 5, if it's 1, index 0, if it's 2-4, return index 1, if it's 5-6 
        #return index 2; each number has a proportional weight = 3
        # because it's ordered, we can use binary search
        
        
        
        # generate a random number
        v = random.randint(0, self.w[-1]-1)
        #bfs search
        ind = self.bfs(self.w, v)
        
        #for i in range(0, self.w[-1]):
        #    ind = self.bfs(self.w, i)
            #print ("i : ", i, ind)
        return ind
    
    def bfs(self, w, v):
        l = 0
        r = len(w)-1
        while (l < r):
            mid = l + (r-l)//2
            if v >= w[mid]:
                l = mid + 1
            else:
                r = mid
        return r                     
        
# Your Solution object will be instantiated and called as such:
# obj = Solution(w)
# param_1 = obj.pickIndex()




```


 ### Example of Rejection sampling 710. Random Pick with Blacklist

 The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/random-pick-with-blacklist/)

```
from collections import defaultdict
from random import random

class Solution(object):

    def __init__(self, N, blacklist):
        """
        :type N: int
        :type blacklist: List[int]
        """
        self.N = N
        blacklist = blacklist

        self.M = len(blacklist)
        
        up_indx = self.N - self.M 
        upper_set = set(range(up_indx, self.N))
        
        upper_diff_list = list(upper_set - set(blacklist))
        
        
        # get the map
        self.hash_map = defaultdict(int)
        i = 0
        for e in blacklist:
            if e < up_indx:
                self.hash_map[e] = upper_diff_list[i]
                i += 1
        
        # print ("hash_map: ", hash_map)
        
        
    def pick(self):
        """
        :rtype: int
        """
        
        # use hashamp  , blacklist has M elemnet,  find white list N-M element,
        # in [0, N-M] element, map element in blacklist to other  [N-M, N)
        
        # figure out the element in [N-M, N] but not in blacklist,  we use set difference
        
        # get a random
        num = randint(0, self.N-self.M) 
        if num in self.hash_map:
            return self.hash_map[num]
        return num


```