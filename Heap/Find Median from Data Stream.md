# 81. Find Median from Data Stream

Numbers keep coming, return the median of numbers at every time a new number added.

## Example
Example 1
```
Input: [1,2,3,4,5]
Output: [1,1,2,2,3]
Explanation:
The medium of [1] and [1,2] is 1.
The medium of [1,2,3] and [1,2,3,4] is 2.
The medium of [1,2,3,4,5] is 3.
```
Example 2
```
Input: [4,5,1,3,2,6,0]
Output: [4,4,4,3,3,3,3]
Explanation:
The medium of [4], [4,5], [4,5,1] is 4.
The medium of [4,5,1,3], [4,5,1,3,2], [4,5,1,3,2,6] and [4,5,1,3,2,6,0] is 3.
```
## Challenge
Total run time in O(nlogn).

## Clarification
What's the definition of Median?

The median is not equal to median in math.
Median is the number that in the middle of a sorted array. If there are n numbers in a sorted array A, the median is A[(n - 1) / 2]A[(n−1)/2].
For example, if A=[1,2,3], median is 2. If A=[1,19], median is 1.

```python
import heapq
class Solution:
    """
    @param nums: A list of integers
    @return: the median of numbers
    """
    def medianII(self, nums):
        n = len(nums)
        res, maxHeap, minHeap = [],[],[]
      
        for i in range(n):
            if not maxHeap or nums[i] <= -maxHeap[0]:
                heapq.heappush(maxHeap, -nums[i])
            else:
                heapq.heappush(minHeap, nums[i])
                
            if len(maxHeap) < len(minHeap):
                heapq.heappush(maxHeap, -heapq.heappop(minHeap))
            elif len(minHeap) < len(maxHeap) - 1:
                heapq.heappush(minHeap, -heapq.heappop(maxHeap))
            res.append(-maxHeap[0])
        return res


```
