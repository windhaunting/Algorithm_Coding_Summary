

## Heap (Priority queue)
Heap is a tree-based structure which is essentially an almost complete tree that satisfies the heap property.
It includes min and max heap.


* Min heap is the parent node p of a node c is greater than or equal to its children nodes
The top is the minimum node.
Vice versa, tHe max heap's top is the maximum node.


* In Python, the heapq library implements min heap in default. 
If we want to use max heap, we could negate node values in the heapq.

Also the Queue.PriorityQueue also implements minheap function.

Queue.PriorityQueue is a thread-safe class, while the heapq module makes no thread-safety guarantees.


## Self-defined sorting in heapq

 How to make self-defined sorting in heap

```
# Here is one example:

from heapq import *


class Element:

  def __init__(self, number, frequency, sequenceNumber):
    self.number = number
    self.frequency = frequency
    self.sequenceNumber = sequenceNumber

  def __lt__(self, other):       #less than,  you could define other   __lt__, __gt__,  __eq__
    # higher frequency wins
    if self.frequency != other.frequency:
      return self.frequency > other.frequency
    # if both elements have same frequency, return the element that is pushed later
    return self.sequenceNumber > other.sequenceNumber


class FrequencyStack:
  sequenceNumber = 0
  maxHeap = []
  frequencyMap = {}

  def push(self, num):
    self.frequencyMap[num] = self.frequencyMap.get(num, 0) + 1
    heappush(self.maxHeap, Element(
      num, self.frequencyMap[num], self.sequenceNumber))            # here we use self-defined sorting Element class. instead, we could also use a tuple
      
    self.sequenceNumber += 1

  def pop(self):
    num = heappop(self.maxHeap).number
    # decrement the frequency or remove if this is the last number
    if self.frequencyMap[num] > 1:
      self.frequencyMap[num] -= 1
    else:
      del self.frequencyMap[num]

    return num

```


## Leetcode 215. Kth Largest Element in an Array


The decription is given here: "https://leetcode.com/problems/kth-largest-element-in-an-array/"

```
# python heaq only support min-heap;
        #if built-in heapq  (priority queue) is allowed in python,
        h = []
        for n in nums:          #only store k element in the heap
            if len(h) < k:
                heapq.heappush(h, n)     #minheap
            elif n > h[0]:
                    heapq.heappop(h)
                    heapq.heappush(h, n)    
            
        return h[0]
```

## Leetcode 295. Find Median from Data Stream

The decription is given here: "https://leetcode.com/problems/find-median-from-data-stream/"

```

# use two heap, max and min heap to find the median values of two data stream


from heapq import heappop
from heapq import heappush

class MedianFinder(object):

    def __init__(self):
        """
        initialize your data structure here.
        """
        
        #use two heaps     # the first one is the max heap , the second one is the min heap
        self.max_hp = []
        self.min_hp = []
        

    def addNum(self, num):
        """
        :type num: int
        :rtype: None
        """
        if  not self.max_hp:
            # put into max_hp
            heappush(self.max_hp, -1*num)
        elif -1*self.max_hp[0] >= num:
            # put into max heap
            heappush(self.max_hp, -1*num)
        elif -1*self.max_hp[0] < num:
            # put into min heap
            heappush(self.min_hp, num)

        # make it balanced
        while len(self.max_hp) > (len(self.min_hp) + 1):
            ele = heappop(self.max_hp)
            heappush(self.min_hp, -1*ele)
            
        while len(self.min_hp) > (len(self.max_hp)):
            ele = heappop(self.min_hp)
            heappush(self.max_hp, -1*ele)
            

    def findMedian(self):
        """
        :rtype: float
        """
        #print("self hp: ", self.max_hp, self.min_hp)
        if not self.max_hp:
            return 0
        
        if not self.min_hp:
            return -1*self.max_hp[0]
        
        if len(self.max_hp) == len(self.min_hp):
            return (-1*self.max_hp[0] + self.min_hp[0])/2.0
        else:
            return -1*self.max_hp[0]
```


