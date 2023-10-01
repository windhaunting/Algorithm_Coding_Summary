
# Sliding window

Sliding window is a technique to reduce the use of nested loop and replace it with a single loop. I would consider it as a special case of two pointers.


### Sliding window example: 76. Minimum Window Substring
Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "". The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/minimum-window-substring/)


```

from collections import Counter

class Solution:
    def minWindow(self, s: str, t: str) -> str:
        
        # use hashmap and sliding window
        dict_cnt = Counter(t)
        cnt = len(t)
        
        l = 0
        
        ans_l = 0
        ans_r = 0
        
        len_s = len(s)
        missing = len(t)
        
        for r in range(0, len_s):
            
            #if dict_cnt[s[r]] > 0:
            missing -= dict_cnt[s[r]] > 0
            
            dict_cnt[s[r]] -= 1
            
            # print("missing :", missing)
            if not missing:
                while(l < r and dict_cnt[s[l]] < 0):

                    dict_cnt[s[l]] += 1
                    l += 1
                
                # print("l, r: ", l, r)
                if not ans_r or (r - l) < (ans_r - ans_l):
                    ans_r = r+1
                    ans_l = l
                    
            print("ans: ", ans_r, ans_l)
        return s[ans_l:ans_r]
```


### Sliding window example: 3. Longest Substring Without Repeating Characters
Given a string s, find the length of the longest substring without repeating characters. The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/longest-substring-without-repeating-characters/)


```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        
        # greedy solution not the optimal solution
        #  "jxdlnaaij", "bbtablud", "pwwkew", "tmmzuxt"
        
        if not s or len(s) == 0:
            return 0
        dict_cnt = {} # collections.defaultdict(int)
        
        i = 0
        j = 0
        prev_indx = i
        ans = 0
        
        while (j < len(s)):
            
            if s[j] in dict_cnt:
                           
                prev_indx = dict_cnt[s[j]]
                ans = max(ans, j-i)
                
                print ("i,j: ", ans, i, j, s[j], prev_indx)
                if prev_indx >= i:
                    i = prev_indx+1
               
            
            dict_cnt[s[j]] = j
            j += 1
        print ("ans: ", ans, i, j)

        ans = max(ans, j-i)
        return ans
        ```
