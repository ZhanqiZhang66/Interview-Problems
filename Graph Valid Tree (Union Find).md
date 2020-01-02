# 178. Graph Valid Tree

Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

```python
class Solution:
    """
    @param n: An integer
    @param edges: a list of undirected edges
    @return: true if it's a valid tree, or false
    """
    def validTree(self, n, edges):
        if n - 1 != len(edges):
            return False
        
        self.father = {i: i for i in range(n)}
        self.num = n
        
        for a, b in edges:
            self.connect(a, b)
            
        return self.num == 1
        
    def connect(self, a, b):
        root_a = self.find(a)
        root_b = self.find(b)
        if root_b != root_a:
            self.father[root_b] = root_a
            self.num -= 1
    """
    @return: An integer
    """
    def query(self):
        return self.num
        
    def find(self, x):
        j = x
        while self.father[j] != j:
            j = self.father[j]
        while x != j:
            fx = self.father[x]
            self.father[x] = j 
            x = fx
        return j
```
