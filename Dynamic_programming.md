
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



### DP example 375. Guess Number Higher or Lower II. 
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


### DP example: 91. Decode Ways
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


### DP example: 322. Coin Change

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


### DP example: 518. Coin Change 2

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

