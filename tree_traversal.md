

## Binary tree traversal algorithms


There are four common methods: 
* Preorder traversal
* Inorder traversal
* PostOrder traversal, 
* Level order traversal


## Recursive ways:

```
## Inorder traversal Method
# A function to do inorder tree traversal
def printInorder(root):

    if root:

        # First recur on left child
        printInorder(root.left)

        # then print the data of node
        print(root.val),

        # now recur on right child
        printInorder(root.right)
     
 
# A function to do postorder tree traversal
def printPostorder(root):

    if root:

        # First recur on left child
        printPostorder(root.left)

        # the recur on right child
        printPostorder(root.right)

        # now print the data of node
        print(root.val),


# A function to do preorder tree traversal
def printPreorder(root):

    if root:

        # First print the data of node
        print(root.val),

        # Then recur on left child
        printPreorder(root.left)

        # Finally recur on right child
        printPreorder(root.right)   


```   

## Iterative ways:  

```
'''
a fancy way is to use a print or visit signal to record the when to print or visit

reference:
    https://www.youtube.com/watch?v=cIoGuSCU14A
 
e.g.
   1
 2   3
4 5
 

use a queue

print 1
visit 2
visit 3

initialize the root as visit first
to put in the stack first
then pop the stack
if it is print, print:
else:put the left and right child into the stack as visiting
and put the root in the stack as print

'''

    def preorderTraversal(self, root: TreeNode) -> List[int]:
     
     
        # 1st recursive
        #SUCCESS 2nd iterative way
     
     
        if root is None:
            return []
        stk = [(root, 0)]        # 1 is print or output, 0 is visiting
     
        lst_res = []
        while(len(stk)):
         
            # dequeue
            cur, flag = stk.pop(-1)
       
            if flag:
                lst_res.append(cur.val)
            else:
                if cur.right:
                    stk.append((cur.right, 0))
                 
                if cur.left:
                    stk.append((cur.left, 0))
                 
                stk.append((cur, 1))        # change to print
 
        return lst_res
 
 
 
     
def inorderTraversal(self, root: TreeNode) -> List[int]:
     
     
        # 1st recursive     
     
        #SUCCESS 2nd iterative way
     
        if root is None:
            return []
        stk = [(root, 0)]        # 1 is print, 0 is visiting
     
        lst_res = []
        while(len(stk)):
         
            # dequeue
            cur, flag = stk.pop(-1)
         
            if flag:
                lst_res.append(cur.val)
            else:
                if cur.right:
                    stk.append((cur.right, 0))
                 
                stk.append((cur, 1))        # change to print
             
                if cur.left:
                    stk.append((cur.left, 0))
 
        return lst_res



```

##  Another iterative methods are based on stacks, which are more complex.

```
class Solution(object):
    def preorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if not root:
            return []
        stack = []
        res = []
        node = root
        while node or stack:
            while node:
                stack.append(node)
                res.append(node.val)
                node = node.left
            node = stack.pop()
            node = node.right
         
        return res
    
# Inroder traverse
class Solution(object):
    def inorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if not root:
            return []
        stack = []
        res = []
        node = root
        while node or stack:
            while node:
                stack.append(node)
             
                node = node.left
            node = stack.pop()
            res.append(node.val)
            node = node.right
         
        return res


# post order
class Solution(object):
    def postorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        res = []
        if not root:
            return res
        stack = []
        pre = None
        while root or stack:
            while root:
                stack.append(root)
                root = root.left
         
            peak = stack[-1]
            if peak.right and peak.right != pre: # ???????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????
                root = peak.right
            else: # ??????????????????????????????????????????????????????????????????????????????????????????????????????????????? ??????????????????????????????????????????????????????

                stack.pop()
                res.append(peak.val)
                pre = peak
        return res

# reverse inorder and post order
class Solution:
    # @param root, a tree node
    # @return a list of integers
    def postorderTraversal(self, root):
        if not root: return []
        ans,q=[],[]
        q.append(root)
        while q:
            cur=q.pop()       
            if cur.left: q.append(cur.left)
            if cur.right: q.append(cur.right)
            ans.append(cur.val)
        return ans[::-1]
 
 
 
# for inorder traverse, we can use Morris traversal without using stack or recursion.
# the space complexity is O(1)
     
#  https://www.educative.io/edpresso/what-is-morris-traversal
Morris Inorder Traversal 
class Node:
 def __init__(self, data):
  self.data = data
  self.left_node = None
  self.right_node = None

def Morris(root):
 # Set current to root of binary tree
 curr = root
 
 while(curr is not None):
  
  if curr.left_node is None:
   print curr.data,
   curr = curr.right_node
  else:
   # Find the previous (prev) of curr
   prev = curr.left_node
   while(prev.right_node is not None and prev.right_node != curr):
    prev = prev.right_node

   # Make curr as right child of its prev
   if(prev.right_node is None):
    prev.right_node = curr
    curr = curr.left_node
    
   # fix the right child of prev
   else:
    prev.right_node = None
    print curr.data
    curr = curr.right_node
   

root = Node(4)
root.left_node = Node(2)
root.right_node = Node(5)
root.left_node.left_node = Node(1)
root.left_node.right_node = Node(3)

Morris(root)
```

