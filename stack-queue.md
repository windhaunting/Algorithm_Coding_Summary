

# Stack and queue
Stack is a linear data structure that follows the order of Last In First Out (LIFO) or First In Last Out(FILO)
Queue is a linear data structure that follows the order of First In First Out (FIFO) or Last In Last Out(LILO)

The stack and queue structures are mandatory knowledge points.  Sometimes the monotonic stack and monotonic queue are
common in the interviews.


### Min Stack

# Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.


class MinStack(object):

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stk = []

    def push(self, x):
        """
        :type x: int
        :rtype: void
        """
        curMin = self.getMin()
        if curMin == None or x < curMin:
            curMin = x

        self.stk.append((x, curMin))
        

    def pop(self):
        """
        :rtype: void
        """
        if len(self.stk) != 0:
            self.stk.pop()
        
        

    def top(self):
        """
        :rtype: int
        """
        if len(self.stk) != 0:
            return self.stk[-1][0]
        else:
            return None
        

    def getMin(self):
        """
        :rtype: int
        """
        #print ('ddd : ', self.min)
        if len(self.stk) != 0:
            return self.stk[-1][1]
        else:
            return None
    


### Circular Queue
 One of the benefits of the circular queue is that we can make use of the spaces in front of the queue. In a normal queue, once the queue becomes full, we cannot insert the next element even if there is a space in front of the queue. But using the circular queue, we can use the space to store new values.


```
class MyCircularQueue:

    # idea use head index i the true position in the queue is i%k, the tail index is j, the true index position is j%k
    # in this way, we may not need to consider the circular complexity
    
    def __init__(self, k: int):
        """
        Initialize your data structure here. Set the size of the queue to be k.
        """
        self.MIN = float('-inf')
        self.k = k
        self.que = [self.MIN]*self.k
        self.head = -1
        self.tail = -1
    def enQueue(self, value: int) -> bool:
        """
        Insert an element into the circular queue. Return true if the operation is successful.
        """
        if not self.isFull():
            self.tail += 1
            self.que[self.tail%self.k] = value
            if self.head == -1:
                self.head = 0
            return True
        return False
    def deQueue(self) -> bool:
        """
        Delete an element from the circular queue. Return true if the operation is successful.
        """
        if self.isEmpty():
            return False
        else:
            self.que[self.head%self.k] = self.MIN
            self.head += 1

            #print("self.que: ",  self.que, self.head, self.tail)

            return True

    def Front(self) -> int:
        """
        Get the front item from the queue.
        """
        return self.que[self.head%self.k]

    def Rear(self) -> int:
        """
        Get the last item from the queue.
        """
        return self.que[self.tail%self.k]

    def isEmpty(self) -> bool:
        """
        Checks whether the circular queue is empty or not.
        """
        return self.tail+1 == self.head
    
    def isFull(self) -> bool:
        """
        Checks whether the circular queue is full or not.
        """
        return (self.tail - self.head) == self.k-1
```


### Monotonic stack example -- 739. Daily Temperatures
Given an array of integers temperatures represents the daily temperatures, return an array answer such that answer[i] is the number of days you have to wait after the ith day to get a warmer temperature. If there is no future day for which this is possible, keep answer[i] == 0 instead. The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/daily-temperatures/)


```
        # Use monotonic stack to reduce o(n)
        #     [73,74,75,71,69,72,76,73]
        #  i:   0  1 2  3  4  5  6  7
        # stk:       [ 6, 7
        # ans: [1, 1, 1, 2, 1, 1, 0, 0]
        
        n = len(T)
        stk = []
        ans = [0] * n
        
        for i in range(n):
            while stk and T[stk[-1]] < T[i]:
                ans[stk[-1]] = i - stk[-1]
                #print("i: ", stk)
                stk.pop(-1)
            
            stk.append(i)
        
        return ans

```

### Monotonic queue example -- 239. Sliding Window Maximum


You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.  The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/sliding-window-maximum/)


```
class maxQueue:            # monotonically decreasing queue

    def __init__(self):
        self.que = collections.deque()

    def push(self, num):

        while (len(self.que) and self.que[-1] < num):
            
            self.que.pop()

        self.que.append(num)
    def pop(self):
        self.que.popleft()
        
    def get_max(self):
        return self.que[0]
    
    
class Solution(object):
    def maxSlidingWindow(self, nums, k):

            if not nums or len(nums) == 0:
            return []
        queObj = maxQueue()
        
        curr_max = 0
        res_lst = []
        for j in range(0, len(nums)):
            queObj.push(nums[j])
            
            if j>=k-1:
                res_lst.append(queObj.get_max()) 
                if nums[j-k+1] == queObj.get_max():
                    queObj.pop()    # bigger than k, must pop(0) 
            print ("queObj:", queObj.que)
            
        return res_lst

```

Other monotonic queue problems:
 862. Shortest Subarray with Sum at Least K;  
 1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit



