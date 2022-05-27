# Shortest Path Algorithms

Shortest path in a graph is to find the shortest distance/path from a source to a destination node.
There are some classical algorithms for solving this. Dijkstras algorithm, Bellman–Ford algorithm, and Floyd Warshall.  uniform-cost, and A* algorithms are could also be used. Here we introduce these first several common algorithms.

If the edge weight in the graph is "1", we could use directly simply BFS to traversal to get the shortest distance/path.


## Dijkstra algorithm

   #### 1. Dijkstra with minheap

```
#Input graph structure: edge list
# Transfer to Adjcency Matrix
# Dijkstra alogirthm with minheap, complexity is o(ElogV), but sac complexity is  O(|E|) items on the min_dist queue, high for dense graph

from heapq import heappush
from heapq import heappop
from collections import defaultdict

import sys
 
class Graph():
 
    def __init__(self, edgeList):
        
        #self.graph = [[0 for column in range(vertices)] 
        #              for row in range(vertices)]
        self.graph = defaultdict(dict)
        for frm, to, cost in edgeList:
            self.graph[frm][to] = cost          # bidirectional or undirectional
            self.graph[to][frm] = cost
        self.V = len(self.graph)
        
        #print ("self. V: ", self.V, self.graph)
        
    def printSolution(self, dist):
        print ("Vertex tDistance from Source")
        for node in range(self.V):
            print (node,"d",dist[node])

    def dijkstra(self, src):
        dist_dict = {i: float('inf') for i in range(0, self.V)}
        dist_dict[src] = 0
        
        hp_min_dist = [(0, src)]         # que
        
        visited = set()
        
        while hp_min_dist:
            cur_dist, cur_nd = heappop(hp_min_dist)
            if cur_nd in visited:
                continue
            visited.add(cur_nd)
            
            for nb in self.graph[cur_nd]:  # range(0, len(self.graph[cur_nd])):         # self.graph[cur_nd]
                if nb in visited:      # avoid re-visit again
                    continue
                #print ("cur_nd:", cur_nd, nb)
                new_dist = cur_dist + self.graph[cur_nd][nb]
                
                if new_dist < dist_dict[nb]:
                    dist_dict[nb] = new_dist
                    heappush(hp_min_dist, (new_dist, nb))
                    #self.printSolution(dist_dict)
        #if len(visited) != len(dist_dict):        # has more than one connected component
        #    return -1
        self.printSolution(dist_dict)
       

edgeList = [(0, 1, 4), (0, 7, 8), (1, 7, 11), (7, 8, 7), (1, 2, 8), (2, 8, 2), (8, 6, 6), (7, 6, 1), 
            (2, 3, 7), (6, 5, 2), (2,5,4), (3, 5, 14), (3, 4, 9), (4, 5, 10)]
g  = Graph(edgeList)
g.dijkstra(0)
         
```


#### 2. Dijkstra without heapmap

    ```
    # Input graph structure: Adjacency Matrix
    # Time Complexity of the implementation is O(V^2). If the input graph is 
    # represented using adjacency list, it can be reduced to O(E log V) or O(VlogV) with the help of binary heap. 


    import sys
    
    class Graph2():
    
        def __init__(self, vertices):
            self.V = vertices
            self.graph = [[0 for column in range(vertices)] 
                        for row in range(vertices)]
    
        def printSolution(self, dist):
            print ("Vertex tDistance from Source")
            for node in range(self.V):
                print (node,"t",dist[node])
                
        def minDistance(self, dist, sptSet):
            '''
            find the vertex with 
            # minimum distance value, from the set of vertices 
            # not yet included in shortest path tree
            
            '''
            # Initilaize minimum distance for next node
            min_val = sys.maxsize
    
            # Search not nearest vertex not in the 
            # shortest path tree
            for v in range(self.V):
                if dist[v] < min_val and sptSet[v] == False:
                    min_val = dist[v]
                    min_index = v
    
            return min_index
        
                
        def dijkstra(self, src):
            '''
            implements Dijkstra's single source 
            # shortest path algorithm for a graph represented 
            # using adjacency matrix representation
            '''
            dist = [sys.maxsize] * self.V
            dist[src] = 0
            
            sptSet = [False] * self.V     # unvisited list
            
            for count in range(self.V):
                
                # Pick the minimum distance vertex from 
                # the set of vertices not yet processed. 
                # u is always equal to src in first iteration
                
                u = self.minDistance(dist, sptSet)
                
                # Put the minimum distance vertex in the 
                # shotest path tree
                sptSet[u] = True
                
                # Update dist value of the adjacent vertices 
                # of the picked vertex only if the current 
                # distance is greater than new distance and
                # the vertex in not in the shotest path tree
                
                for v in range(self.V):
                    if self.graph[u][v] > 0 and sptSet[v] == False:
                        if dist[v] > dist[u] + self.graph[u][v]:
                            dist[v] = dist[u] + self.graph[u][v]
            self.printSolution(dist)
            
                
    g  = Graph2(9)
    g.graph = [[0, 4, 0, 0, 0, 0, 0, 8, 0],
            [4, 0, 8, 0, 0, 0, 0, 11, 0],
            [0, 8, 0, 7, 0, 4, 0, 0, 2],
            [0, 0, 7, 0, 9, 14, 0, 0, 0],
            [0, 0, 0, 9, 0, 10, 0, 0, 0],
            [0, 0, 4, 14, 10, 0, 2, 0, 0],
            [0, 0, 0, 0, 0, 2, 0, 1, 6],
            [8, 11, 0, 0, 0, 0, 1, 0, 7],
            [0, 0, 2, 0, 0, 0, 6, 7, 0]
            ];
    
    g.dijkstra(0)

    ```



## Bellman-Ford algorithm

```
# Bellman-Ford algorithm  O(VE)     
#Dijkstra doesn’t work for Graphs with negative weight edges,
#Bellman-Ford works for such graphs. Bellman-Ford is also simpler than Dijkstra and suites well for distributed systems.
#But time complexity of Bellman-Ford is O(VE), which is more than Dijkstra.
         
from collections import defaultdict

#
class Graph2:
    def __init__(self, vertices):
        self.V = vertices   # No of vertices
        self.graph = []  # edge list 
        
    def addEdge(self, u, v, w):
        '''
        add an edge to graph
        '''
        self.graph.append([u, v, w])
        
    def printArr(self, dist):
        '''
        print shortest path result from one src
        '''
        print ("Vertex Distance from Source")
        for i in range(self.V):
            print ( (i, dist[i]))
    
    def BellmanFord(self, src):
        '''
         finds shortest distances from src to
         all other vertices using Bellman-Ford algorithm.  The function
         also detects negative weight cycle
        '''
        #1st initialize distance from src to allo ther nodes
        dist = [float("Inf")] * self.V
        dist[src] = 0
        
        
        #2nd Relax all edges |V| - 1 times. A simple shortest 
        # path from src to any other vertex can have at-most |V| - 1 
        # edges
        for i in range(self.V - 1):
            # Update dist value and parent index of the adjacent vertices of
            # the picked vertex. Consider only those vertices which are still in
            # queue
            
            for u, v, w in self.graph:
                if dist[u] != float("Inf") and dist[u] + w < dist[v]:
                    dist[v] = dist[u] + w
                # dist[v] = min(dist[v], dist[u] + w)        # or use this line
                
        # 3rd: check for negative-weight cycles. (a circle has all the negative weights).   The above step 
        # guarantees shortest distances if graph doesn't contain 
        # negative weight cycle.  If we get a shorter path, then there
        # is a cycle.
        
        for u, v, w in self.graph:
            if dist[u] != float("Inf") and dist[u] + w < dist[v]:
                print ("Graph contains negative weight cycle")
                return
                
        # print all distance
        self.printArr(dist)
         
         
g = Graph2(5)
g.addEdge(0, 1, -1)
g.addEdge(0, 2, 4)
g.addEdge(1, 2, 3)
g.addEdge(1, 3, 2)
g.addEdge(1, 4, 2)
g.addEdge(3, 2, 5)
g.addEdge(3, 1, 1)
g.addEdge(4, 3, -3)
 
#Print the solution
print ("test 1:  ")
g.BellmanFord(0)
 
g = Graph2(5)
g.addEdge(0, 1, -1)
g.addEdge(0, 2, 4)
g.addEdge(2, 2, -5)
         
#Print the solution  test
print ("test 2:  ")
g.BellmanFord(0)

```



## Floyd Warshall Algorithm 

```

#Good for Multiple source to multiple target shortest paths

# Number of vertices in the graph 
V = 4 
  
# Define infinity as the large enough value. This value will be 
# used for vertices not connected to each other 
INF  = 99999
  
# Solves all pair shortest path via Floyd Warshall Algorithm 
def floydWarshall(graph): 
  
    """ dist[][] will be the output matrix that will finally 
        have the shortest distances between every pair of vertices """
    """ initializing the solution matrix same as input graph matrix 
    OR we can say that the initial values of shortest distances 
    are based on shortest paths considering no  
    intermediate vertices """
    dist = map(lambda i : map(lambda j : j , i) , graph) 
      
    """ Add all vertices one by one to the set of intermediate 
     vertices. 
     ---> Before start of an iteration, we have shortest distances 
     between all pairs of vertices such that the shortest 
     distances consider only the vertices in the set  
    {0, 1, 2, .. k-1} as intermediate vertices. 
      ----> After the end of a iteration, vertex no. k is 
     added to the set of intermediate vertices and the  
    set becomes {0, 1, 2, .. k} 
    """
    for k in range(V): 
  
        # pick all vertices as source one by one 
        for i in range(V): 
  
            # Pick all vertices as destination for the 
            # above picked source 
            for j in range(V): 
  
                # If vertex k is on the shortest path from  
                # i to j, then update the value of dist[i][j] 
                dist[i][j] = min(dist[i][j] , 
                                  dist[i][k]+ dist[k][j] 
                                ) 
    printSolution(dist) 
  
  
# A utility function to print the solution 
def printSolution(dist): 
    print ("Following matrix shows the shortest distances between every pair of vertices")
    
    for i in range(V): 
        for j in range(V): 
            if(dist[i][j] == INF): 
                print ("%7s" %("INF")), 
            else: 
                print ("%7d\t" %(dist[i][j])), 
            if j == V-1: 
                print ("")
  
  
# Driver program to test the above program 
# Let us create the following weighted graph 
""" 
            10 
       (0)------->(3) 
        |         /|\ 
      5 |          | 
        |          | 1 
       \|/         | 
       (1)------->(2) 
            3           """
graph = [[0,5,INF,10], 
             [INF,0,3,INF], 
             [INF, INF, 0,   1], 
             [INF, INF, INF, 0] 
        ] 
# Print the solution 
floydWarshall(graph); 

```