# 120. Triangle
## Description
Given a `triangle` array, return the minimum path sum from top to bottom.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index `i` on the current row, you may move to either index `i` or index `i + 1` on the next row.

**Example:**
```
Input: triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
Output: 11
Explanation: The triangle looks like:
   2
  3 4
 6 5 7
4 1 8 3
The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11.
```
## My Solution
This is a well-formed problem and can be solved by dynamic programming easily.

I used a 2D vector `dp` to store the minimum paths at each position.

I update the `dp` in a top-down way. For every `dp[i][j]`, we can update `dp[i+1][j]` and `dp[i+1][j+1]` as the following 

```C++
dp[i+1][j] = min(dp[i+1][j], dp[i][j] + triangle[i+1][j]
dp[i+1][j+1] = min(dp[i+1][j], dp[i][j] + triangle[i+1][j+1]
```

```C++
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        vector<vector<int>> dp = triangle;
        
        for(auto& row : dp)
            for(auto& element : row)
                element = INT_MAX;
        dp[0][0] = triangle[0][0];
        
        for(int i = 0; i < n - 1; i++) {
            for(int j = 0; j < triangle[i].size(); j++) {
                dp[i+1][j] = min(dp[i+1][j], dp[i][j] + triangle[i+1][j]);
                dp[i+1][j+1] = min(dp[i+1][j+1], dp[i][j] + triangle[i+1][j+1]);
            }
        }
        
        int res = INT_MAX;
        for(int i = 0; i < dp[n-1].size(); i++)
            res = min(res, dp[n-1][i]);
        
        return res;
    }
```

## Classic Solution
An optimized way to implement the dynamic programming is use the bottom-up traversal instead of top-down, which uses only a 1D vector.

We start from the nodes on the bottom row; the min pathsums for these nodes are the values of the nodes themselves. From there, the min pathsum at the ith node on the kth row would be the lesser of the pathsums of its two children plus the value of itself, i.e.:
```C++
minpath[k][i] = min( minpath[k+1][i], minpath[k+1][i+1]) + triangle[k][i];
```

Or even better, since the row `minpath[k+1]` would be useless after `minpath[k]` is computed, we can simply set `minpath` as a 1D array, and iteratively update itself:
```C++
// For the kth level:
minpath[i] = min( minpath[i], minpath[i+1]) + triangle[k][i]; 
```

```C++
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        vector<int> dp = triangle.back();
        
        for(int i = n - 2; i >= 0; i--) {
            for(int j = 0; j < triangle[i].size(); j++) {
                dp[j] = min(dp[j], dp[j+1]) + triangle[i][j];
            }
        }
        return dp[0];
    }
```