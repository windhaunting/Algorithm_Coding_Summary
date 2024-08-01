## Linked list 
Linked list is a linear data structure where data point to next in a sequence. 
There are singly linked list, circular linked list, doubly linked list.

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
        while(p1 and p2):
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
# Design a data structure that follows the constraints of a Least Recently Used (LRU) cache (Leetcode 146).

from collections import defaultdict

class Node:
    def __init__(self, key, val):
        self.key = key
        self.val = val
        self.prev = None
        self.next = None
        
class LRUCache:

    def __init__(self, capacity: int):
        self.key_map = defaultdict(Node)
        self.cap = capacity
        self.dummy_head = Node(-2**32, -2**32)
        self.dummy_end = Node(2**32, 2**32)
        
        self.dummy_head.next = self.dummy_end
        self.dummy_end.prev = self.dummy_head
        self.cur_no = 0  
        
    def append_node(self,node):
        # p<->dummy_end =>    p<->node<->dummy_end
        
        prev_node = self.dummy_end.prev
        prev_node.next = node
        node.prev = prev_node
        node.next = self.dummy_end
        self.dummy_end.prev = node
        
        
    def pop_node(self, node):
        # p<->node<->next   => p<->next
        prev_node = node.prev
        nxt_node = node.next
        prev_node.next = nxt_node
        nxt_node.prev = prev_node
        

    def get(self, key: int) -> int:
        if key not in self.key_map:
            return -1
        
        node = self.key_map[key]
        self.pop_node(node)
        self.append_node(node)
        
        return node.val
        

    def put(self, key: int, value: int) -> None:
        node = Node(key, value)
        
        if self.cur_no < self.cap and key not in self.key_map:
            self.append_node(node)
            self.cur_no += 1
        elif key in self.key_map:
            old_node = self.key_map[key]
            self.pop_node(old_node)
            self.append_node(node)
        elif self.cur_no == self.cap:
            deleted_node = self.dummy_head.next
            del self.key_map[deleted_node.key]
            self.pop_node(deleted_node)
            self.append_node(node)
        self.key_map[key] = node
