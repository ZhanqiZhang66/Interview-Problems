# 589 connecting graph (Union Find Implementation)
```python
class ConnectingGraph:
    """
    @param: n: An integer
    """
    def __init__(self, n):
        self.father = {i: i for i in range(1,n+1)}
        #self.size = {i: i for i in range(1,n+1)}
        
    """
    @param: a: An integer
    @param: b: An integer
    @return: nothing
    """
    def connect(self, a, b):
        root_a = self.find(a)
        root_b = self.find(b)
        if root_b != root_a:
            self.father[root_b] = root_a
            #self.size[root_b] += self.size[root_a]
            # write your code here

    """
    @param: a: An integer
    @param: b: An integer
    @return: A boolean
    """
    def query(self, a, b):
        root_a = self.find(a)
        root_b = self.find(b)
        return root_a == root_b
        
    def find(self, x):
        j = x
        while self.father[j] != j :
            j = self.father[j] # all the way up
        while x != j: # on the way up, for each node x
            fx = self.father[x] #save the father node
            self.father[x] = j #set father to root
            x = fx #x all the way up
        return j
```
