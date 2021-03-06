# Connecting Graph 2 (Union Find)
```python
class ConnectingGraph2:
    """
    @param: n: An integer
    """
    def __init__(self, n):
        # do intialization if necessary
        self.father = {i: i for i in range(1, n+1)}
        self.size = {i: 1 for i in range(1, n+1)}

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
            self.size[root_a] += self.size[root_b]

    """
    @param: a: An integer
    @return: An integer
    """
    def query(self, a):
        # write your code here
        root_a = self.find(a)
        return self.size[root_a]
        
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
