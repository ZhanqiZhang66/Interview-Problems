# 364. Trapping Rain Water II
Given n x m non-negative integers representing an elevation map 2d where the area of each cell is 1 x 1, compute how much water it is able to trap after raining.

這題的概念很簡單
想像你要倒水進一個凹槽
首先,你必須先把四周圈起來, 因為四周不能儲水
接著為了防止水滲出來, 從凹槽裡最低的地方開始倒進去
接著每經過一個低窪, 就把低窪的水填到跟目前的高度一樣
然後繼續往下水位低的地方倒
拿題目的範例演示一下：
```
[12,13, 0,12]
[13, 4,13,12]
[13, 8,10,12]
[12,13,12,12]
[13,13,13,13]
```
首先, 先把四周(除了O以外的位置)倒進minheap
接著把四周最低的點從minheap裡pop出來
[12,13,“0”,12] <= 在(0,2)的0被選出來(最小)
[13, O, O,12]
[13, O, O,12]
[12, O, O,12]
[13,13,13,13]
0被pop出來, 因為沒人比0低,所以0下方的13被heappush進去後結束, water=0
接著在(2,3)的12被pop出來
```
[ X, X, X, X]
[ X, 4, X, X]
[ X, 8,10,“12”] <=在(2,3)的12被選出來(最小)
[ X,13,12, X]
[ X, X, X, X]
```
在12的四周查找, 發現10這個位置沒用過, 把水補到目前最小值12後加入minheap
此時water=2
接著在(2,2)的12被pop出來
```
[ X, X, X, X]
[ X, 4, X, X]
[ X, 8,12, X] <=在(2,2)的12被選出來(最小)
[ X,13,12, X]
[ X, X, X, X]
```
在(2,2)的12的四周查找, 發現8這個位置沒用過, 把水補到目前最小值12後加入minheap
8被倒入4的水變成12放入minheap
然後下方的12也要放入minheap
此時water加了4以後變成6
```
[ X, X, X, X]
[ X, 4, X, X]
[ X,12, X, X] <=在(2,1)的12被選出來(最小)
[ X,13, X, X]
[ X, X, X, X]
```
在(2,1)的12的四周查找, 發現4這個位置沒用過, 把補到目前最小值12後加入minheap
然後4被倒入8的水變成12放入minheap
然後下方的13也要放入minheap, 但由於13比12高, 所以不必計算
此時water加了8變成14
```
[ X, X, X, X]
[ X,12, X, X]
[ X, X, X, X] <=在(2,1)的12被選出來(最小)
[ X,13, X, X]
[ X, X, X, X]
```
最後12跟13對water貢獻都是+0, 算完後結束循環！
Time: O(mnlog(m+n))
Space: O(mn) <= 需要visited
```python
import heapq
class Solution:
    """
    @param heights: a matrix of integers
    @return: an integer
    """
    def trapRainWater(self, heights):
        # write your code here
        if len(heights) == 0 or len(heights[0]) == 0:
            return 0
        Direction = [(1,0), (-1, 0), (0, 1), (0, -1)]

        h = len(heights)
        w = len(heights[0])
        done = {}
        minHeap = [] 
        res = 0
        
        for i in range(h):
            for j in range(w):
                done[(i,j)] = 0
                if i == 0 or i == h-1 or j == 0 or j == w-1:
                    heapq.heappush(minHeap, (heights[i][j], i, j))
                    done[(i,j)] = 1
        while minHeap:
            (min_height, x, y) = heapq.heappop(minHeap)
            for (dx, dy) in Direction:
                nx = x + dx
                ny = y + dy
                if 0 <= nx < h and 0 <= ny < w and not done[(nx,ny)]:
                    nh = max(heights[nx][ny], min_height)
                    res += nh - heights[nx][ny]
                    heapq.heappush(minHeap, (nh, nx, ny))
                    done[(nx,ny)] = 1
        return res

```
