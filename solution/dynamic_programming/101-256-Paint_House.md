# 256. Paint House

## Description
There are a row of `n` houses, each house can be painted with one of the three colors: red, blue or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that **no two adjacent houses have the same color**.

The cost of painting each house with a certain color is represented by a `n x 3` cost matrix. For example, `costs[0][0]` is the cost of painting house `0` with color red; `costs[1][2]` is the cost of painting house `1` with color green, and so on... Find the minimum cost to paint all houses.

Note:
All costs are positive integers.

**Example:**
```
Input: [[17,2,17],[16,16,5],[14,3,19]]
Output: 10
Explanation: Paint house 0 into blue, paint house 1 into green, paint house 2 into blue. 
             Minimum cost: 2 + 5 + 3 = 10.
```

## My Solution
This problem is similar to [House Robber](https://leetcode.com/problems/house-robber/) problem. Instead of simply choose rob a house or not, we need to decide which color we will use.

As a result, we need to keep the state for each color, which indicates we should use a 2D vector to store the dp state.

Suppose `dp[i][0]`, `dp[i][1]` and `dp[i][2]` indicates the minimum costs found so far to paint the house `i` in `red`, `blue` and `green` respectively. Then the state transition function is easy to derive:
```C++
    dp[i][0] = min(dp[i-1][1], dp[i-1][2]) + costs[i][0];
    dp[i][1] = min(dp[i-1][0], dp[i-1][2]) + costs[i][1];
    dp[i][2] = min(dp[i-1][0], dp[i-1][1]) + costs[i][2];
```
And here's the code:
```c++
    int minCost(vector<vector<int>> &costs) {
        int n = costs.size();
        if(n < 1)
            return 0;
        vector<vector<int>> dp(costs);
        for(int i = 1; i < n; i++) {
            dp[i][0] = min(dp[i-1][1], dp[i-1][2]) + costs[i][0];
            dp[i][1] = min(dp[i-1][0], dp[i-1][2]) + costs[i][1];
            dp[i][2] = min(dp[i-1][0], dp[i-1][1]) + costs[i][2];
        }

        return min({dp[n-1][0], dp[n-1][1], dp[n-1][2]});
    }
```
**Optimization**
Since `dp[i][]` only depends on `dp[i-1][]`, so we don't need the whole table but only three variables to record the minimum costs for each color so far is enough.
```C++
    int minCost(vector<vector<int>> &costs) {
        int prev_red = 0;
        int prev_blue = 0;
        int prev_green = 0;

        for(int i = 0; i < costs.size(); i++) {
            prev_red = min(prev_blue, prev_green) + costs[i][0];
            prev_blue = min(prev_red, prev_green) + costs[i][1];
            prev_green = min(prev_red, prev_blue) + costs[i][2];
        }
        return min({prev_red, prev_blue, prev_green});
    }
```