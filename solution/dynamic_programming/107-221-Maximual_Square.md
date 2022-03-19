# 221. Maximum Square

## Description
Given an `m x n` binary `matrix` filled with `0`'s and `1`'s, find the largest square containing only `1`'s and return its area.

**Example:**
```
Input: matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
Output: 4
```

## Classic Solution
This question can be solved using DP, but the state transition function is to easy to get.

Suppose `dp[i][j]` represents the maximum length of the `1`-square containing `matrix[i][j]`. \

Then for the basic case, `dp[i][j] = (matrix[i] == '1')` when `i` or `j` is zero. That is because the length of the `1`-square is no larger than `1`.

For the general case:

- if `matrix[i][j] == '0'`, then `dp[i][j] = 0`.
- if `matrix[i][j] == '1'`, then `dp[i][j] = min{ dp[i-1][j], dp[i][j-1], dp[i-1][j-1] }`.

This is quite tricky to find this transition function.

```C++
int maximalSquare(vector<vector<char>>& matrix) {
    int m = matrix.size();
    int n = matrix.front().size();
    int maxLen = 0;
    
    vector<vector<int>> dp(m, vector<int>(n, 0));
    for(int i = 0; i < m; i++) {
        for(int j = 0; j < n; j++) {
            if(!i || !j)
                dp[i][j] = (matrix[i][j] == '1');
            else
                dp[i][j] = matrix[i][j] == '0' ?  0 : min({dp[i-1][j], dp[i][j-1], dp[i-1][j-1]}) + 1;
            
            maxLen = max(maxLen, dp[i][j]);
        }
    }
    return maxLen * maxLen;
}
```

**Optimization**
We can use a 1D array in replace of the 2D matrix:

```C++
int maximalSquare(vector<vector<char>>& matrix) {
    int m = matrix.size();
    int n = matrix.front().size();
    int maxLen = 0;
    
    vector<int> dp(n, 0);
    for(int i = 0; i < m; i++) {
        int currUpLeft = 0;
        for(int j = 0; j < n; j++) {
            int nextUpLeft = dp[j];
            if(!i || !j)
                dp[j] = (matrix[i][j] == '1');
            else
                dp[j] = matrix[i][j] == '0' ? 0 : min({ dp[j-1], dp[j], currUpLeft }) + 1;
            
            maxLen = max(maxLen, dp[j]);
            currUpLeft = nextUpLeft;
        }
    }
    return maxLen * maxLen;
}
```