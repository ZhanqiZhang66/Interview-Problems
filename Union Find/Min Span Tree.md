# 629 Minimum Spanning Tree

Given a list of Connections, which is the Connection class (the city name at both ends of the edge and a cost between them), find edges that can connect all the cities and spend the least amount.
Return the connects if can connect all the cities, otherwise return empty list.

Example
Example 1:

Input:
["Acity","Bcity",1]
["Acity","Ccity",2]
["Bcity","Ccity",3]
Output:
["Acity","Bcity",1]
["Acity","Ccity",2]
Example 2:

Input:
["Acity","Bcity",2]
["Bcity","Dcity",5]
["Acity","Dcity",4]
["Ccity","Ecity",1]
Output:
[]

### Notice
Return the connections sorted by the cost, or sorted city1 name if their cost is same, or sorted city2 if their city1 name is also same.
```python
'''
Definition for a Connection
class Connection:

    def __init__(self, city1, city2, cost):
        self.city1, self.city2, self.cost = city1, city2, cost
'''
'''
1. Sort all the edges in non-decreasing order of their weight.
2. Pick the smallest edge. Check if it forms a cycle with the spanning tree formed so far. If cycle is not formed, include this edge. Else, discard it.
3. Repeat step#2 until there are (V-1) edges in the spanning tree.
'''
class MinSpanTree:
    def __init__(self):
        self.father = {}
        self.size = 0
        
    def couldConnect(self, a, b):
        root_a = self.find(a) #root 
        root_b = self.find(b)
        if root_b == root_a :
            return False
        else:
            self.father[root_b] = root_a
            self.size -= 1 #every time we connect with an edge, we lose a cluster
            return True

    def find(self, x):
        path = []
        while x != self.father[x]:
            path.append(x)
            x = self.father[x] #x now becomes the father
        for p in path:
            self.father[p] = x
        return x   
class Solution:
    # @param {Connection[]} connections given a list of connections
    # include two cities and cost
    # @return {Connection[]} a list of connections from results
    def lowestCost(self, connections):
        # Write your code here
        if not connections or len(connections) == 0: 
            return []
            
        res = []
        connections.sort(key = lambda x: (x.cost, x.city1, x.city2))
        tree = MinSpanTree()
        
        for connection in connections: #put every node into tree
            cities = (connection.city1, connection.city2)
            for city in cities:
                if city not in tree.father:
                    tree.father[city] = city
                    tree.size += 1  # this will eventually be the number of verticies (clusters)
        for connection in connections:
            if tree.couldConnect(connection.city1, connection.city2):
                res.append(connection)
        
        if tree.size == 1: #if everything is connected into one cluster
            return res 
        return []
                
            
        
        
```
