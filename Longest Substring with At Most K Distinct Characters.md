# Longest Substring with At Most K Distinct Characters
```python
from collections import defaultdict
class Solution:
    """
    @param s: A string
    @param k: An integer
    @return: An integer
    """
    def lengthOfLongestSubstringKDistinct(self, s, k):
        # write your code here
        n = len(s)
        r = 0
        counter =defaultdict(int) # when the char is not in the map, the default appearence is 0
  
        longest = 0
       
        if not s or k == 0:
            return 0
            
        for l in range(n):
            while r < n and (len(counter) < k or s[r] in counter):
                counter[s[r]] += 1
                r += 1 
            longest = max(longest, r - l)
            if counter[s[l]] > 1:
                counter[s[l]] -= 1 
            else:
                del counter[s[l]]
          
        return longest
```
