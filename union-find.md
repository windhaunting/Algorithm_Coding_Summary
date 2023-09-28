
# Union-Find

 Union-Find is a disjoint-set data structure is a data structure that keeps track of a set of elements
 partitioned into a number of disjoint (non-overlapping) subsets. It performs two useful operations on such a data structure, Find--Determine which subset a particular element is in. This can be used for determining if two elements are in the same subset Union--join two subsets into a single subset.

Application:  Detect a cycle in an undirected graph; Find the number of connected components



### Implementation one of Union-Find

```
from collections import defaultdict
  
#This class represents a undirected graph using adjacency list representation
class Graph:
  
    def __init__(self):
        self.V= set()    # store the vertices
        self.graph = defaultdict(list) # default dictionary to store graph
  
 
    # function to add an edge to graph
    def addEdge(self,u,v):
        self.graph[u].append(v)
        self.V.add(u)
        self.V.add(v)
        
    def union(self, parentArr, a, b):     # not path compression
        '''
        union a, b
        '''
        pa = self.findParent(parentArr, a)
        pb = self.findParent(parentArr, b)
        parentArr[pa] = pb
            
        
    def findParent(self, parentArr, a):
        '''
        find parent of an element a;     like the root of a tree
        '''
        if parentArr[a] != a:
            parentArr[a] = self.findParent(parentArr, parentArr[a])
        return parentArr[a]
        
        
    def checkCycle(self):
        '''
        check self.graph has cycle or not
        '''
        
        parentArr = [i for i in range(0, len(self.V))]
        
        for i in self.graph:
            for j in self.graph[i]:
                pi = self.findParent(parentArr, i)
                pj = self.findParent(parentArr, j)
                if pi == pj:
                    return True
                self.union(parentArr, pi, pj)
        return False
        
g = Graph()
g.addEdge(0, 1)
g.addEdge(1, 2)
g.addEdge(2, 3)
g.addEdge(1, 3)

print ("cycle or not ", g.checkCycle())

```

### Implementation one of Union-Find--optimized implementation


```
# 2nd optimized implementation ; union-find and path compression; make it balanced

# A union by rank and path compression based  
# program to detect cycle in a graph 
from collections import defaultdict 
  
# a structure to represent a graph 
class Graph: 
      
    def __init__(self, num_of_v): 
        self.num_of_v = num_of_v 
        self.edges = defaultdict(list) 
          
    # graph is represented as an  
    # array of edges 
    def add_edge(self, u, v): 
        self.edges[u].append(v) 
  
  
class Subset: 
    def __init__(self, parent, rank): 
        self.parent = parent 
        self.rank = rank 
  
# A utility function to find set of an element 
# node(uses path compression technique) 
def find(subsets, node): 
    if subsets[node].parent != node: 
        subsets[node].parent = find(subsets, subsets[node].parent) 
    return subsets[node].parent 
  
# A function that does union of two sets  
# of u and v(uses union by rank) 
def union(subsets, u, v): 
    
    #if find(subsets, u) == find(subsets, v):
    #    return 
    
    # Attach smaller rank tree under root  
    # of high rank tree(Union by Rank) 
    if subsets[u].rank > subsets[v].rank: 
        subsets[v].parent = u 
    elif subsets[v].rank > subsets[u].rank: 
        subsets[u].parent = v 
          
    # If ranks are same, then make one as  
    # root and increment its rank by one 
    else: 
        subsets[v].parent = u 
        subsets[u].rank += 1
  
# The main function to check whether a given 
# graph contains cycle or not 
def isCycle(graph): 
      
    # Allocate memory for creating sets 
    subsets = [] 
  
    for u in range(graph.num_of_v): 
        subsets.append(Subset(u, 0)) 
  
    # Iterate through all edges of graph,  
    # find sets of both vertices of every  
    # edge, if sets are same, then there 
    # is cycle in graph. 
    for u in graph.edges: 
        u_rep = find(subsets, u) 
  
        for v in graph.edges[u]: 
            v_rep = find(subsets, v) 
  
            if u_rep == v_rep: 
                return True
            else: 
                union(subsets, u_rep, v_rep) 
  
# Driver Code 
g = Graph(3) 
  
# add edge 0-1 
g.add_edge(0, 1) 
  
# add edge 1-2 
g.add_edge(1, 2) 
  
# add edge 0-2 
g.add_edge(0, 2) 
  
if isCycle(g): 
    print('Graph contains cycle') 
else: 
    print('Graph does not contain cycle') 

# Ref: https://www.geeksforgeeks.org/union-find-algorithm-set-2-union-by-rank/

```

### Example Union-Find: 305. Number of Islands II
Extend from Leetcode 200--Number of Islands, we may perform an addLand operation which turns the water at position (row, col) into a land. 

```
class Solution(object):
    def numIslands2(self, m, n, positions):
        ans = []
        islands = Union()
        for p in map(tuple, positions):
            islands.add(p)
            for dp in (0, 1), (0, -1), (1, 0), (-1, 0):
                q = (p[0] + dp[0], p[1] + dp[1])
                if q in islands.group:
                    islands.union(p, q)
            ans += [islands.count]
        return ans

class Union(object):
    def __init__(self):
        self.group = {}
        self.island = {}
        self.count = 0

    def add(self, p):
        self.group[p] = p
        self.island[p] = 1
        self.count += 1

    def find(self, i):
        if i == self.group[i]:
            return i
        else:
            return self.find(self.group[i])

    def union(self, p, q):
        root1, root2 = self.find(p), self.find(q)
        if root1 == root2:
            return
        if self.island[root1] > self.island[root2]:
            root1, root2 = root2, root1
        self.group[root1] = root2
        self.island[root2] += self.island[root1]
        self.count -= 1
```


### Example Union-Find: 1562. Find Latest Group of Size M

The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/find-latest-group-of-size-m/)


```
class UnionFindSet:
    def __init__(self, n):
        self.parents = list(range(n))
        self.ranks = [0] * n
        
    def find(self, u):
        if u != self.parents[u]:
            self.parents[u] = self.find(self.parents[u])
        return self.parents[u]
    
    def union(self, u, v):
        pu, pv = self.find(u), self.find(v)
        if pu == pv:
            return False
        if self.ranks[pu] > self.ranks[pv]:
            self.parents[pv] = pu
            self.ranks[pu] += self.ranks[pv]
        elif self.ranks[pv] > self.ranks[pu]:
            self.parents[pu] = pv
            self.ranks[pv] += self.ranks[pu]
        else:
            self.parents[pu] = pv
            self.ranks[pv] += self.ranks[pu]
        return True

class Solution:
    def findLatestStep(self, arr: List[int], m: int) -> int:
        n, ans = len(arr), -1
        uf = UnionFindSet(n)
        
        for step, i in enumerate(arr):
            i -= 1
            uf.ranks[i] = 1
            for j in (i - 1, i + 1):
                if 0 <= j < n:
                    if uf.ranks[uf.find(j)] == m:
                        ans = step
                    if uf.ranks[j]:
                        uf.union(i, j)
        
        for i in range(n):
            if uf.ranks[uf.find(i)] == m:
                return n
            
        return ans

```