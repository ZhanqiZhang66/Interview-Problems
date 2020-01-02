# 384 Longest Substring without Repeating Characters
```python
    def lengthOfLongestSubstring(self, s):
        # write your code here
        
        l = 0 ; r = 0
        existed = set([])
        longest = 0
        for l in range(len(s)):
            while r < len(s) and s[r] not in existed:
                existed.add(s[r])
                r += 1
            longest = max(longest, r - l)
            existed.remove(s[l])
        
        return longest       
```
