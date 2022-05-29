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


 ### Example of Rejection sampling 470. Implement Rand10() Using Rand7()
 The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/implement-rand10-using-rand7/)

```
class Solution(object):
    def rand10(self):
        """
        :rtype: int
        """
        
        # [7,7] =>49  [1, 40]  =>[41, 49]
        # for [1, 40], we use [1,2,3,...,10], [1,2,3,..10],...[1,2,3,...,10]
        
        while(True):
            a = rand7()
            b = rand7()
            
            num = (a-1)*7 + b
            
            if num <= 40:
                break
        return 1 + (num-1)%10

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