# 465 Kth Smallest Sum In Two Sorted Arrays
```python
import heapq
class Solution:
    """
    @param A: an integer arrays sorted in ascending order
    @param B: an integer arrays sorted in ascending order
    @param k: An integer
    @return: An integer
    """
    def kthSmallestSum(self, A, B, k):
        # write your code here
        w1 = len(A)
        w2 = len(B)
        if not A or not B: return None
        visited = set([0])
        minHeap = [(A[0]+ B[0], 0, 0)]
        val = None
        for _ in range(k):
            val, x, y = heapq.heappop(minHeap)
            if x + 1 < w1 and (x + 1)* w2 + y not in visited:
                heapq.heappush(minHeap, (A[x+1] + B[y], x+1, y))
                visited.add((x + 1)* w2 + y)
            if y + 1 < w2 and x *w2 + y + 1 not in visited:
                heapq.heappush(minHeap, (A[x] + B[y+1], x, y+1))
                visited.add(x *w2 + y + 1)
            #print(minHeap)
        return val   
```
