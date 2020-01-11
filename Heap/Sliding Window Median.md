# 360. Sliding Window Median
Given an array of n integer, and a moving window(size k), move the window at each iteration from the start of the array, find the median of the element inside the window at each moving. (If there are even numbers in the array, return the N/2-th number after sorting the element in the window. )

## Example
Example 1:
```
Input:
[1,2,7,8,5]
3
Output:
[2,7,7]
```
Explanation:
At first the window is at the start of the array like this `[ | 1,2,7 | ,8,5]` , return the median `2`;
then the window move one step forward.`[1, | 2,7,8 | ,5]`, return the median `7`;
then the window move one step forward again.`[1,2, | 7,8,5 | ]`, return the median `7`;
Example 2:
```
Input:
[1,2,3,4,5,6,7]
4
Output:
[2,3,4,5]
```
Explanation:
At first the window is at the start of the array like this `[ | 1,2,3,4, | 5,6,7]` , return the median `2`;
then the window move one step forward.`[1,| 2,3,4,5 | 6,7]`, return the median `3`;
then the window move one step forward again.`[1,2, | 3,4,5,6 | 7 ]`, return the median `4`;
then the window move one step forward again.`[1,2,3,| 4,5,6,7 ]`, return the median `5`;
## Challenge
O(nlog(n)) time
```python
import heapq
class Solution:
    """
    @param nums: A list of integers
    @param k: An integer
    @return: The median of the element inside the window at each moving
    """
    def medianSlidingWindow(self, nums, k):
        if not nums or k == 0: return []
        res, self.maxHeap, self.minHeap = [], [], []
        n = len(nums)
        for i in range(n):
            if not self.maxHeap or nums[i] <= -self.maxHeap[0]:
                heapq.heappush(self.maxHeap, -nums[i])
            else:
                heapq.heappush(self.minHeap, nums[i])
            
            if i >= k:
                if nums[i-k] <= -self.maxHeap[0]:
                    self.maxHeap.remove(-nums[i-k])
                    heapq.heapify(self.maxHeap)
                else:
                    self.minHeap.remove(nums[i-k])
                    heapq.heapify(self.minHeap)
            self.balance()        
            if i >= k-1:
                res.append(-self.maxHeap[0])
        return res
        
    def balance(self):
        while len(self.maxHeap) < len(self.minHeap):
            heapq.heappush(self.maxHeap, -heapq.heappop(self.minHeap))
        while len(self.minHeap) < len(self.maxHeap) - 1:
            heapq.heappush(self.minHeap, -heapq.heappop(self.maxHeap))
    #This is O(n) solution using the default remove function         
```
