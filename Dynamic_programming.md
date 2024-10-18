
Dynamic Programming (DP) is an algorithmic technique for solving an optimization problem by breaking it down into simpler subproblems and utilizing the fact that the optimal solution to the overall problem depends upon the optimal solution to its subproblems. It is guaranteed that Dynamic Programming will generate an optimal solution as it generally considers all possible cases and then choose the best.

## The characteristics of DP problems.

1) Optimal substructure
2) Overlapping subproblem
3) Previous substructure does not affect the latter one
4) Memorization of small subproblem solution to save space


## What types of problem could be possible to be solved by DP?
1) min, Max
2) count(*)
3) yes/no (to arrive)?
。。。


## How to start with a DP problem:
    
1) Initialize state
2) Define a transition function with the characteristics of DP
3) Top-down method with memoization  or bottom-up method 


## DP common type

1) Sequence DP 
2) 2 Sequence DP 
3) Matrix DP 
4) Knapsack problem (0-1 knapsack)
5) Range DP;



### Another problem to notice:'
How to reconstruct the optimal path from DP. e.g. List the Difference Between Two Strings


### DP example 70. Climbing Stairs (Sequence DP)
You are climbing a staircase. It takes n steps to reach the top.Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top? The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/climbing-stairs/description/)

```
class Solution(object):
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """

        '''
        def climbStairsHelper(n, memo):
  
            # Base cases
            if n == 1:
                return 1
            if n == 2:
                return 2
            
            if n in memo:
                return memo[n]
            
            # Recursively calculate and store the result in the memo dictionary
            memo[n] = climbStairsHelper(n - 1, memo) + climbStairsHelper(n - 2, memo)
            
            return memo[n]
        '''

        #1st using recursive  top-down with memorization
        memo = {}
        return climbStairsHelper(n, memo)
        
        '''
        #2nd DP  bottom up  d[n] = d[n-1] + d[n-2]       #2 1 step + 2 steps; using memoization
        d = [0]*(n + 1)
        d[0] = 1
        d[1] = 1
        
        for i in range(2, n+1):
            
            d[i] = d[i-1] + d[i-2]
            
        print ("d  : ", n, d[n])
        return d[n]
        '''

        
        #further optimize space
        if n <=1:
            return n
        a = 1
        b = 1
        for i in range(2, n+1):
            c = a + b
            a = b
            b = c
        return b
```


### DP example: 91. Decode Ways  (Sequence DP)
Given a string s containing only digits, return the number of ways to decode it. 
The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/decode-ways/)


```

"""
Bottom-up method:

class Solution:
    def numDecodings(self, s: str) -> int:
        
        if s is None or len(s) == 0:
            return 0
        dp = [0] * (len(s) + 1)
        dp[0] = 1

        for i in range(1, len(s)+1):
            if int(s[i-1]) != 0:
                dp[i] += dp[i-1]
            if i-2 >= 0 and 9 < int(s[i-2:i]) <= 26:     # '06' => 0   '30'=> 0;   '209' => 2
                dp[i] += dp[i-2]
        return dp[len(s)]
"""


# top-down method:
class Solution:
    def numDecodings(self, s: str) -> int:
        
        n = len(s)
        #@lru_cache(maxsize=None)
        def solve(i):
            if i>=n: return 1
            a, b = 0, 0
            if '1'<=s[i]<='9': a = solve(i+1)
            if i+1 < n and '10'<=s[i]+s[i+1]<='26': b = solve(i+2)
            return a+b
        return solve(0)

```

# DP Example: 1143. Longest Common Subsequence (2 Sequence DP)

Given two strings text1 and text2, return the length of their longest common subsequence. If there is no common subsequence, return 0.
The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/longest-common-subsequence/description/)  

```
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        
        
        # bottom-up tabulation
        
        
        m = len(text1)
        
        n = len(text2)
        
        DP = [[0 for _ in range(n+1)] for _ in range(m+1)]
        
        for i in range(1, m+1):
            for j in range(1, n+1):
 
                if text1[i-1] == text2[j-1]:
                    DP[i][j] = 1 + DP[i-1][j-1]
                    
                else:
                    DP[i][j] = max(DP[i-1][j], DP[i][j-1])
                    
        return DP[m][n]
```

### DP example 375. Guess Number Higher or Lower II. (Matrix/Range DP) 
The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/guess-number-higher-or-lower-ii/#/description)


```
'''
 (1) Define the subproblem
       define DP[i][j]  the money needs to pay to guarantee a win, so the answer is DP[1][n]
(2) find the recursion
     the number is in [1, n] ,  guess the number x (from 1 to n).  if we pick up k,   if k != x
     then the worst case complexity to guarantee win is the get the maximum cost:  k + max(dp[1][k-1]), dp[k+1][n])
for all x from 1 to n, the the money needs to pay is dp[i][j] = min(dp[i][j], k + max(dp[1][k-1]), dp[k+1][n]))
(3) find the base case
     the base case is dp[i][j] = 0 for i == j  from 1 to n
'''
'''
# 1st DP bottom-up
'''
def getMoneyAmount(self, n):
    dp = [[0] * (n+1) for i in range(0, n+1)]
        
    for i in range(n-1, 0, -1):                 #from n-1 to 1
        for j in range(i+1, n+1):               #from i+1 to n
            dp[i][j] = 2**64
            for k in range(i, j):             #from i to j-1
                dp[i][j] = min(dp[i][j], k + max(dp[i][k-1], dp[k+1][j]))
                #print ("dp ij ", i, j, k, dp[i][j])
    return dp[1][n]
'''
# 2nd DP top-down optimized
def getMoneyAmount(self, n):
    def guesshelper(dp, i, j):
        if i >= j: return 0
        if dp[i][j] > 0: return dp[i][j]
            
        dp[i][j] = float("inf")
            
        for k in range(i, j+1):            
            dp[i][j] = min(dp[i][j], k + max(guesshelper(dp, i, k-1), guesshelper(dp, k+1, j)))
            
        return dp[i][j]
        
    dp = [[0] * (n + 1) for i in range(n + 1)]
    return guesshelper(dp, 1, n)

```

### DP example: 322. Coin Change  (Matrix DP) 

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money. Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1. You may assume that you have an infinite number of each kind of coin. The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/coin-change/) 


```

import sys
class Solution:
    def coinChange(self, coins, amount):
        """
        :type coins: List[int]
        :type amount: int
        :rtype: int
        """
        
        '''
        # Method 1: using top-down recursive: count(coins, amount) =  1 + min(count(coins, amount - coins[i])) for i from 0 to len(coins)
        
        # time limit;   with input [1,2,5]   100;         lots of repetition; overlapping subproblem
        if amount == 0:
            return 0
        ans = sys.maxsize
        
        for i in range(0, len(coins)):
            if coins[i] <= amount:
                tmp = self.coinChange(coins, amount - coins[i])
                if tmp != -1 and tmp + 1 < ans:
                    ans = tmp + 1
        return ans if ans != sys.maxsize else -1
        '''

        """
        # Method 2 use DP to optimize;     memoization; dp[i] indicate the minimum number of coins for i value
        """
        #dp[i][amount]  index i make change of amount
        # dp[i][c] = min(dp[i-1][c], 1 + dp[i][c-coins[i]]) if i-1>=0,  c-coins[i] >= 0
        # initialize
        #dp[i][0] == 1,  0<=i<=len(coins)
        #dp[0][j] == 0
        """
        """
        n = len(coins)
        dp = [[float('inf') for _ in range(amount+1)] for _ in range(n)]
              
        for i in range(0, n):
            dp[i][0] = 0

              
        for i in range(0, n):
              for c in range(1, amount+1):
                if i-1 >= 0:
                    dp[i][c] = dp[i-1][c]
                if c >= coins[i] and dp[i][c-coins[i]] != float('inf'):
                    dp[i][c] = min(dp[i][c], 1 + dp[i][c-coins[i]])
        
        return dp[n-1][amount] if dp[n-1][amount] != float('inf') else -1
        """
    
        # Method 2's optimized space to o(amount)
        dp = [0] * (amount + 1)
        for i in range(1, amount+1):
            dp[i] =  sys.maxsize
        
        for i in range(1, amount+1):
            for j in range(len(coins)):
                if i >= coins[j]:
                    dp[i] = min(dp[i], 1 + dp[i-coins[j]])
        return dp[amount]  if dp[amount]  != sys.maxsize else -1

```


### DP example: 518. Coin Change 2 (Matrix DP) 

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money. Return the number of combinations that make up that amount. If that amount of money cannot be made up by any combination of the coins, return 0. You may assume that you have an infinite number of each kind of coin. The detailed description is [<span style="color:blue;"> here </span>](https://leetcode.com/problems/coin-change-2/)  


```
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        
        # 1st success 
        N = len(coins)
        if N == 0: 
            return int(N == amount)
        
        dp_sum = [[0] * (amount+1) for _ in range(N)]
        for i in range(N): 
            dp_sum[i][0] = 1
        
        for i,j in product(range(N), range(amount)):
            
            dp_sum[i][j+1] = dp_sum[i-1][j+1]
            
            if j+1 - coins[i] >= 0:
                dp_sum[i][j+1] += dp_sum[i][j+1-coins[i]]           
                    
        return dp_sum[-1][-1]
        """
        
        # 2nd optimize
        
        dp = [0] * (amount + 1)
        dp[0] = 1
        for c in coins:
            for j in range(1, amount + 1):
               if j >= c:
                   dp[j] += dp[j - c]
        return dp[amount]
        """

```


### DP example:  List the Difference Between Two Strings  (2 Sequence DP and Reconstruction)

Given two strings of uppercase letters source and target, list (in string form) a sequence of edits to convert from source to target that uses the fewest edits possible.
For example, with strings source = "ABCDEFG", and target = "ABDFFGH" we might return: ["A", "B", "-C", "D", "-E", "F", "+F", "G", "+H"

```
from typing import List

def diff_between_two_strings(source: str, target: str) -> List[str]:
    #pass # your code goes here
    # overlapping subproblem, optimal substructure
    '''
    A B CDEFG
    A B DFFGH
    if equal, then skip,
    not equal, either -C or +D when not matching.
    dp[i][j] = 1+min(dp[i-1][j], dp[i][j-1])
    dp[i][0] == i      # i from 0 to m
    dp[0][j] == j      # j from 0 to n
    '''
    # bottom-up method
    m, n = len(source), len(target)
    
    # Initialize dp table
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    # Fill base cases
    for i in range(1, m + 1):
        dp[i][0] = i  # Deleting all characters from source
    for j in range(1, n + 1):
        dp[0][j] = j  # Inserting all characters into target

    # Fill the dp table
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if source[i - 1] == target[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]  # No change needed
            else:
                dp[i][j] = min(dp[i - 1][j] + 1,   # Deletion
                               dp[i][j - 1] + 1)   # Insertion

    # Backtrack to find the sequence of edits
    edits = []
    i, j = m, n
    while i > 0 or j > 0:
        if i > 0 and j > 0 and source[i - 1] == target[j - 1]:
            edits.append(source[i - 1])
            i -= 1
            j -= 1
        elif i > 0 and (j == 0 or dp[i - 1][j] + 1 == dp[i][j]):
            # Deletion from source
            edits.append(f"-{source[i - 1]}")
            i -= 1
        elif j > 0 and (i == 0 or dp[i][j - 1] + 1 == dp[i][j]):
            # Insertion to target
            edits.append(f"+{target[j - 1]}")
            j -= 1
     

    return edits[::-1]  # Reverse the list to get the correct order

# debug your code below
print(diff_between_two_strings("ABCDEFG", "ABDFFGH"))

```

Others:
LC 494. Target Sum
LC 53. Maximum Subarray



