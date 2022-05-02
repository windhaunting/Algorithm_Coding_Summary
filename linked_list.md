## Linked list 
Linked list is a linear data structure where data points to next in a sequence. 
There are singly linked list, circly linked list, doubly linked list.

The common coding knowledge points for linked list:

* Reverse linked list
* Slow and fast pointer for Linked list


## Reverse linked list

```
# Reverse linked list
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reverseList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        #1st iterative way
        '''
        if head is None or head.next is None:
            return head
        p1 = head
        p2 = p1.next
        
        head.next = None;
        while(p1 is not None and p2 is not None):
            tmp = p2.next
            p2.next = p1
            p1 = p2
            p2 = tmp
 
        return p1
        '''    
            
        # 2nd recursive way:
        if head is None or head.next is None:
            return head
        p1 = head.next
        head.next = None
        p2 = self.reverseList(p1)
        p1.next = head         
        return p2
```


## Slow and Fast pointer and reverse linked list

```
# Given a singly linked list of integers, determine whether or not it's a palindrome.
in O(N) time and O(1) space

        # get the first and right half part with slow and fast pointer; then reverse the half part; then compare the first half;
        if head is None or head.next is None:
            return True
        slow = head
        fast = head
        while(fast.next and fast.next.next):
            slow = slow.next
            fast = fast.next.next
            
        last = slow.next
        while(last.next):
            nextTwo = last.next             #   1->2->3  ->   4 ->  3 ->  2 -> 1
            last.next = nextTwo.next        #              slow  last  nextTwo
            nextTwo.next = slow.next        # not last
            slow.next = nextTwo            # => 1->2->3  ->   4 ->  1 ->  2 -> 3
            
        print('last: ', last.val)
        pre = head
        while(slow.next and pre.next):
            print ('mid val: ', slow.next.val)
            if pre.val != slow.next.val:
                return False
            pre = pre.next
            slow = slow.next
        
        return True

```


## LRU cache

```
# Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.

class Node:
    def __init__(self, key, val):
        self.key = key
        self.val = val
        self.prev = None
        self.next = None


class LRUCache:

    def __init__(self, capacity: int):
        self.cap = capacity
        self.head = None
        self.tail = None
        self.dict_cache = {}      # store the Node as the value
        
    def get(self, key: int) -> int:     
        if key not in self.dict_cache:
            return -1
        
        #move to tail
        #remove the node
        t = self.dict_cache[key]
        self.removeNode(t)
        self.appendNode(t)
        
        return t.val
    
    def put(self, key: int, value: int) -> None:
        
        if key in self.dict_cache:
            t = self.dict_cache[key]
            t.val = value
            # move to tail
            self.removeNode(t)
            self.appendNode(t)
        else:
            if self.cap <= len(self.dict_cache):
                # remove head
                del self.dict_cache[self.head.key]
                self.removeNode(self.head)

            # add to tail
            t = Node(key, value)
            self.appendNode(t)
            self.dict_cache[key] = t # store the node
        
        
    def removeNode(self, nd) -> None:
        if nd.prev is None:
            self.head = nd.next
        else:
            nd.prev.next = nd.next

        if nd.next is None:
            self.tail = nd.prev
        else:
            nd.next.prev = nd.prev
            
    def appendNode(self, nd) -> None:

        if self.tail is not None:
            self.tail.next = nd

        nd.prev = self.tail
        nd.next = None
        self.tail = nd

        if self.head is None:
            self.head = self.tail

```
