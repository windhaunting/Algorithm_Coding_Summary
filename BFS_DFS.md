# BFS and DFS

BFS (Breadth First Search) is vertex-based technique for finding the shortest path in the graph. It uses a Queue data structure that follows first in first out. In BFS, one vertex is selected at a time when it is visited and marked then its adjacent are visited and stored in the queue.

DFS (Depth First Search) is an edge-based technique. It uses the Stack data structure and performs two stages, first visited vertices are pushed into the stack, and second if there are no vertices then visited vertices are popped. 

The Time complexity of BFS or DFS is O(V + E) when Adjacency List is used and O(V^2) when Adjacency Matrix is used, where V stands for vertices and E stands for edges.

Extension from BFS, there is a Directional BFS if we have or could utilize two sources.



#### BFS on the graph--iterative code with queue:

```

from collections import defaultdict
 
#Class to represent a graph
class Graph:
    def __init__(self):
        self.graph = defaultdict(list) # dictionary containing adjacency List
        self.vertices = set() # vertices
 
    # function to add an edge to graph
    def addEdge(self,u,v):
        self.graph[u].append(v)
        if u not in self.vertices:
            self.vertices.add(u)
        if v not in self.vertices:
            self.vertices.add(v)
            
            
        
    def bfsIterative(self, src):
        '''
        iterative traversal dfs with stack
        '''
        visited = [False] * (len(self.graph))
        
        que = []   # queue
        que.append(src)
        visited[src] = True
        
        while (len(que)):
            v = que.pop(0)       # last element
            print ("visited: ", v)
            for n in self.graph[v]:
                if not visited[n]:
                    que.append(n)
                    visited[n] = True
        
        
# Create a graph given in the above diagram
g = Graph()
g.addEdge(0, 1)
g.addEdge(0, 2)
g.addEdge(1, 2)
g.addEdge(2, 0)
g.addEdge(2, 3)
g.addEdge(3, 3)

```


#### BFS on the tree--iterative code with queue:

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None


class Solution(object):
    def findBottomLeftValue(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        
        #2nd  SUCCESS,  use iterative 
        
        res = [root, 1]
        que = [(root, 1)]
        while (len(que)):
            # pop
            ele = que.pop(0)
            node = ele[0]
            depth = ele[1]
            
            if depth > res[1]:
                #update
                res[0] = node
                res[1] = depth 
            if node.left is not None:
                que.append((node.left, depth+1))
            if node.right is not None:
                que.append((node.right, depth+1))
            
        return res[0].val

```


#### DFS on the graph--iterative with stack, and recursive code:


```
# 1st recursive way
# 2nd iterative way

from collections import defaultdict
 
#Class to represent a graph
class Graph:
    def __init__(self):
        self.graph = defaultdict(list) # dictionary containing adjacency List
        self.vertices = set() # vertices
 
    # function to add an edge to graph
    def addEdge(self,u,v):
        self.graph[u].append(v)
        if u not in self.vertices:
            self.vertices.add(u)
        if v not in self.vertices:
            self.vertices.add(v)
            
            
    def dfsRecursive(self, src):
        '''
        recursive dfs
        '''
        visited = [False] * (len(self.vertices))
        self.dfsHelper(src, visited)
            
                    
    def dfsHelper(self, v, visited):
        # visited
        visited[v] = True
        print ("v: ", v)
        
        for n in self.graph[v]:
            if visited[n] == False:
                self.dfsHelper(n, visited)
                
     
        
    #2nd dfs iterative research    
    def dfsIterative(self, src):
        '''
        iterative traversal dfs with stack
        '''
        visited = [False] * (len(self.graph))
        
        stk = []   # stack
        stk.append(src)
        visited[src] = True
        
        while (len(stk)):
            v = stk.pop()       # last element
            print ("visited: ", v)

            for n in self.graph[v]:
                if not visited[n]:
                    stk.append(n)
                    visited[n] = True
        
        
# Create a graph given in the above diagram
g = Graph()
g.addEdge(0, 1)
g.addEdge(0, 2)
g.addEdge(1, 2)
g.addEdge(2, 0)
g.addEdge(2, 3)
g.addEdge(3, 3)
 
print ("Following is DFS recursive from (starting from vertex 2)", g.dfsRecursive(2))
print ("Following is DFS iterative from (starting from vertex 2)", g.dfsIterative(2))

```



#### DFS on the tree--recursive code:


```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def findBottomLeftValue(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        
        # use recursive order traversal dfs 
        
        if root is None:
            return 0
        
        def getTreeDepthHelper(node, depth, res):
            if node.left is None and node.right is None:
                if depth > res[1]:
                    # update depth
                    res[0] = node
                    res[1] = depth
            if node.left is not None:
                getTreeDepthHelper(node.left, depth+1, res)
            if node.right is not None:
                getTreeDepthHelper(node.right, depth+1, res)
            #print ("res: ", depth, res[0].val)
        
        depth = 1
        res = [root, 1]
        getTreeDepthHelper(root, depth, res)
        return res[0].val


```