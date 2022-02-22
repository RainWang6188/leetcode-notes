# 63. Unique Paths II
## Description
A robot is located at the top-left corner of a `m x n` grid.

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid .

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and space is marked as `1` and `0` respectively in the grid.


**Example:**
```
Input: obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
Output: 2
Explanation: There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
```

## My Solution
The solution is just an adpatation of the [previous](https://leetcode.com/problems/unique-paths/description/) question: once we reach an obstacle, the paths to this position will be cleared.

```C++
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {      
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        
        vector<vector<int>> dp(m, vector<int>(n, 0));
        dp[0][0] = 1;
        
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(obstacleGrid[i][j] == 1)
                    dp[i][j] = 0;
                else if(j == 0 && i > 0)
                    dp[i][j] = dp[i-1][j];
                else if(i == 0 && j > 0)
                    dp[i][j] = dp[i][j-1];
                else if(i > 0 && j > 0)
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
```

### Optimization
Actually, we just need an 1D vector to implement the dp just as the previous question.

```C++
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {      
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        
        vector<int> dp(n, 0);
        dp[0] = 1;
        
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(obstacleGrid[i][j] == 1)
                    dp[j] = 0;
                else if(j > 0)
                    dp[j] += dp[j-1];
            }
        }
        return dp.back();
    }
```