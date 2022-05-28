
Topological sorting  for Directed Acyclic Graph (DAG) is a linear ordering of vertices such that for
 every directed edge uv, vertex u comes before v in the ordering. Topological Sorting for a graph is 
 not possible if the graph is not a DAG. Therefore, it could be also used to check whether there is a cycle in a graph.


## Ways to implement topological sorting. 

 ### 1st use recursive way:  use stack


```
    # In topological sorting, we use a temporary stack. We don’t print the vertex immediately,
    # we first recursively call topological sorting for all its adjacent vertices,
    # then push it to a stack. Finally, print contents of stack. Note that a vertex is pushed to stack
    # only when all of its adjacent vertices (and their adjacent vertices and so on) are already in stack.

    # Python program to print topological sorting of a DAG
    from collections import defaultdict
    
    # Class to represent a graph
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
                
        # A recursive function used by topologicalSort
        def topologicalSortHelper(self,v, visited, stack):
    
            # Mark the current node as visited.
            visited.add(v)
    
            # Recur for all the vertices adjacent to this vertex
            for i in self.graph[v]:
                if i not in visited:
                    self.topologicalSortHelper(i,visited,stack)
    
            # Push current vertex to stack which stores result
            stack.insert(0,v)
    
        # The function to do Topological Sort. It uses recursive 
        # topologicalSortUtil()
        def topologicalSortRecursive(self):
            '''
            use stack and 
            '''
            # Mark all the vertices as not visited
            visited = set()
        
            stack =[]
    
            # Call the recursive helper function to store Topological
            # Sort starting from all vertices one by one
            for v in self.vertices:                # for a graph which is not only one connected component
                if v not in visited:
                    self.topologicalSortHelper(v,visited,stack)
    
            # Print contents of stack
            print (stack[::-1])

    g= Graph()
    g.addEdge(7, 2);
    g.addEdge(7, 0);
    g.addEdge(4, 0);
    g.addEdge(4, 1);
    g.addEdge(2, 3);
    g.addEdge(3, 1);       

    print ("Following is a Topological Sort of the given graph")
    g.topologicalSort()

    g2= Graph()

    g2.addEdge(1, 2);
    g2.addEdge(2, 1);
    g2.topologicalSort()

    # reference: https://www.geeksforgeeks.org/topological-sorting/
```


 ### 2nd use iterative way;  use queue

'''
    # A DAG G has at least one vertex with in-degree 0 and one vertex with out-degree 0.

    # A Python program to print topological sorting of a graph
    # using indegrees
    
    #Class to represent a graph
    class Graph2:
        def __init__(self):
            self.graph = defaultdict(list) #dictionary containing adjacency List
            self.vertices = set() # vertices
            
        # function to add an edge to graph
        def addEdge(self,u,v):
            self.graph[u].append(v)
            self.vertices.add(u)
            self.vertices.add(v)
                
        # The function to do Topological Sort. 
        def topologicalSort(self):
            
            # Create a vector to store indegrees of all
            # vertices. Initialize all indegrees as 0.
            inDegree = {v: 0 for v in self.vertices}     #defaultdict(i)      # [0]*(self.V)
            
            # Traverse adjacency lists to fill indegrees of
            # vertices.  This step takes O(V+E) time;  or use addEdge function to put indegree
            for v in self.graph:
                for dst in self.graph[v]:
                    inDegree[dst] += 1
                        
    
            print (" inDegree ", inDegree)
            # Create an queue and enqueue all vertices with
            # indegree 0
            queue = []
            for v in self.vertices:
                if inDegree[v] == 0:
                    queue.append(v)
                    
            #Initialize count of visited vertices
            cnt = 0
    
            # Create a vector to store result (A topological
            # ordering of the vertices)
            top_order = []
    
            # One by one dequeue vertices from queue and enqueue
            # adjacents if indegree of adjacent becomes 0
            while queue:
    
                # Extract front of queue (or perform dequeue)
                # and add it to topological order
                u = queue.pop(0)
                top_order.append(u)
                
                # Iterate through all neighbouring nodes
                # of dequeued node u and decrease their in-degree
                # by 1
                for v in self.graph[u]:
                    inDegree[v] -= 1
                    # If in-degree becomes zero, add it to queue
                    if inDegree[v] == 0:
                        queue.append(v)
    
                cnt += 1
                
            # Check if there was a cycle
            if cnt != len(self.vertices):
                print ("There exists a cycle in the graph, cnt=", cnt)
            else :
                #Print topological order
                print (top_order)
                
                  
    g= Graph2()
    g.addEdge(8, 2);
    g.addEdge(8, 0);
    g.addEdge(4, 0);
    g.addEdge(4, 1);
    g.addEdge(2, 3);
    g.addEdge(3, 1);
    
    print ("Following is a Topological Sort of the given graph")
    g.topologicalSort()

```

##  Example 207. Course Schedule

There are a total of n courses you have to take, labeled from 0 to n-1. Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1] Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses? The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/course-schedule/)


```
Except from DFS, here we could use topological sort BFS get the indegree, get each node's indegree, then use topological sort from a node with indegree == 0
    # if we can not visit all the node, there exists a cycle in the graph
        if not prerequisites or len(prerequisites) == 0:
            return True
        
        from collections import defaultdict
        dict_graph = defaultdict(list)
        dict_indeg = defaultdict(int)
        for eg in prerequisites:
            dict_graph[eg[0]] += [eg[1]]
            dict_indeg[eg[1]] += 1
            
        que = []
        for nd in range(0, numCourses):
            if dict_indeg[nd] == 0:
                que.append(nd)
        cnt = 0  
        #print ("que: ", que, dict_indeg)
        while (len(que)):
            nd = que.pop(0)
            
            cnt += 1
            for nb in dict_graph[nd]:
                dict_indeg[nb] -= 1
                if dict_indeg[nb] == 0:
                    que.append(nb)
            #print ("nd: ", nd)
        if cnt != numCourses:  # there is a cycle
            return False
        return True

```