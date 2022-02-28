# 64. Minimum Path Sum

## Description
Given a `m x n` `grid` filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.

**Example:**
```
Input: grid = [[1,3,1],[1,5,1],[4,2,1]]
Output: 7
Explanation: Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.
```

## My Solution
The state transition function is easy to get.

Suppose `dp[i][j]` denotes the minimum path sum at position `(i,j)`, since we can only move either down or right at any point, we have:
```C++
    dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]
```

As you can see, `dp[i][j]` only depends on `dp[i-1][j]` and `dp[i][j-1]`, we can use a 1D array instead of keeping the whole 2D array: `dp[j] = min(dp[j-1], dp[j]) + grid[i][j]`.

However, we need to pay attention to the corner cases when `i` or `j` is equal to `0` where the left or up element doesn't exist.

```C++
    int minPathSum(vector<vector<int>>& grid) {
        if(grid.empty() || grid.front().empty())    return 0;
        int m = grid.size();
        int n = grid.front().size();
        
        vector<int> dp(grid.front());
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(i == 0 && j == 0)
                    continue;
                else if(i == 0)
                    dp[j] += dp[j-1];
                else if(j == 0)
                    dp[j] += grid[i][j];
                else
                    dp[j] = min(dp[j-1], dp[j]) + grid[i][j];
            }
        }
        return dp.back();
    }
```