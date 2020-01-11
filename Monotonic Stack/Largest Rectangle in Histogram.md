# 122. Largest Rectangle in Histogram
Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

Example
Example 1:
```
Input：[2,1,5,6,2,3]
Output：10
```
Explanation：
The third and fourth rectangular truncated rectangle has an area of 2*5=10.
Example 2:
```
Input：[1,1]
Output：2
```
Explanation：
The first and second rectangular truncated rectangle has an area of 2*1=2.
## 单调栈做法：
1. 首先在数组最后加一个0，方便它能保证把入栈的所有数都pop出来、都分析一遍；
2. 然后循环数组，如果循环到的数比栈顶的数还大，就直接入栈，因为它在栈里的下面一个数就是它的左边界，比他小嘛；
3. 如果循环到的数不比栈顶的数大，则不停地从栈中剔除成员、并对每一个成员进行分析。怎么分析呢？首先当前循环到的数一定是pop成员右边第一个比其小的数，右边界定了，然后根据2可知，它下面的那个数就是它的左边界。左右边界已定，面积可知！
4. 特殊处理的是，如果要pop某个成员、而且它是栈中最后一个成员，则右边界即是 pos = -1 的位置，即从数组的最左边到该成员、没有成员比他还小了。
```python
class Solution:
    """
    @param height: A list of integer
    @return: The area of largest rectangle in the histogram
    """
    def largestRectangleArea(self, height):
        if not height: return 0
        stack, max_area = [], 0
        for hi_idx, h in enumerate(height + [0]):
            while stack and h <= height[stack[-1]]:
                cur_h = height[stack.pop()]
                #if not stack:
                #    continue
                lo_idx = stack[-1] if stack else -1
                #特殊处理的是，如果要pop某个成员、而且它是栈中最后一个成员，则右边界即是 pos = -1 的位置，
                #即从数组的最左边到该成员、没有成员比他还小了
                area = cur_h * (hi_idx - lo_idx - 1)
                #print(stack)
                max_area = max(area, max_area)
            stack.append(hi_idx)
        return max_area
                

```
