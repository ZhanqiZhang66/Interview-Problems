# 401 Kth Smallest Number in Sorted Matrix
```python
import heapq

class Solution:
    """
    @param matrix: a matrix of integers
    @param k: An integer
    @return: the kth smallest number in the matrix
    """
    def kthSmallest(self, matrix, k):
        n = len(matrix)
        if n == 0:
            return None
        
        m = len(matrix[0])
        if m == 0:
            return None
            
        minheap = [(matrix[0][0], 0, 0)]
        visited = set([0])
        num = None
        for _ in range(k):
            num, x, y = heapq.heappop(minheap)
            if x + 1 < n and (x + 1) * m + y not in visited:
                heapq.heappush(minheap, (matrix[x + 1][y], x + 1, y))
                visited.add((x + 1) * m + y)
            if y + 1 < m an d x * m + y + 1 not in visited:
                heapq.heappush(minheap, (matrix[x][y + 1], x, y + 1))
                visited.add(x * m + y + 1)

        return num
```
