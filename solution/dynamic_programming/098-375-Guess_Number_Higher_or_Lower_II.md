# 375. Guess Number Higher or Lower

## Description
We are playing the Guessing Game. The game will work as follows:

1. I pick a number between `1` and `n`.
2. You guess a number.
3. If you guess the right number, you win the game.
4. If you guess the wrong number, then I will tell you whether the number I picked is higher or lower, and you will continue guessing.
5. Every time you guess a wrong number `x`, you will pay `x` dollars. If you run out of money, you lose the game.

Given a particular `n`, return the minimum amount of money you need to guarantee a win regardless of what number I pick.

**Example:**
```
Input: n = 10
Output: 16
Explanation: The winning strategy is as follows:
- The range is [1,10]. Guess 7.
    - If this is my number, your total is $0. Otherwise, you pay $7.
    - If my number is higher, the range is [8,10]. Guess 9.
        - If this is my number, your total is $7. Otherwise, you pay $9.
        - If my number is higher, it must be 10. Guess 10. Your total is $7 + $9 = $16.
        - If my number is lower, it must be 8. Guess 8. Your total is $7 + $9 = $16.
    - If my number is lower, the range is [1,6]. Guess 3.
        - If this is my number, your total is $7. Otherwise, you pay $3.
        - If my number is higher, the range is [4,6]. Guess 5.
            - If this is my number, your total is $7 + $3 = $10. Otherwise, you pay $5.
            - If my number is higher, it must be 6. Guess 6. Your total is $7 + $3 + $5 = $15.
            - If my number is lower, it must be 4. Guess 4. Your total is $7 + $3 + $5 = $15.
        - If my number is lower, the range is [1,2]. Guess 1.
            - If this is my number, your total is $7 + $3 = $10. Otherwise, you pay $1.
            - If my number is higher, it must be 2. Guess 2. Your total is $7 + $3 + $1 = $11.
The worst case in all these scenarios is that you pay $16. Hence, you only need $16 to guarantee a win.
```
## Classic Solution

For each number `x` in range[i~j], we do: 
```C++
result_when_pick_x = x + max{DP([i~x-1]), DP([x+1, j])}
// the max means whenever you choose a number, the feedback is always bad and therefore leads you to a worse branch.
```

Then we get 
```C++
DP([i~j]) = min{xi, ... ,xj}
// this min makes sure that you are minimizing your cost.
```
### 1. Top-down Solution
```C++
class Solution {
public:
    int getMoneyAmount(int n) {
        vector<vector<int>> dp(n+1, vector<int>(n+1, 0));
        return helper(dp, 1, n);
    }
    
private:
    int helper(vector<vector<int>>& dp, int start, int end) {
        if(start >= end)
            return 0;
        
        if(dp[start][end] != 0)
            return dp[start][end];
        
        if(start == end - 1) {
            dp[start][end] = start;
            return start;
        }
        
        int globalCost = INT_MAX;
        for(int k = start + 1; k < end; k++) {
            int localCost = k + max(helper(dp, start, k-1), helper(dp, k+1, end)); 
            globalCost = min(globalCost, localCost);
        }
        dp[start][end] = globalCost;
        
        return globalCost;
    }
};
```

### 2. Bottom-up Solution
We should be carefull when designing the for loops since `dp[i][j]` depends on the internal values in `[i, j]`.

```C++
    int getMoneyAmount(int n) {
        vector<vector<int>> dp(n+1, vector<int>(n+1, 0));
        
        for(int j = 2; j <= n; j++) {
            for(int i = j - 1; i > 0; i--) {
                if(i == j - 1) {
                    dp[i][j] = i;
                    continue;
                }
                
                int globalCost = INT_MAX;
                for(int k = i + 1; k < j; k++) {
                    int localCost = k + max(dp[i][k-1], dp[k+1][j]);
                    globalCost = min(localCost, globalCost);
                }
                dp[i][j] = globalCost;
            }
        }
        
        return dp[1][n];
    }
```