# 265. Paint House II

## Description
There are a row of `n` houses, each house can be painted with one of the `k` colors. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.
The cost of painting each house with a certain color is represented by a `n x k` cost matrix. For example, `costs[0][0]` is the cost of painting house `0` with color `0`; `costs[1][2]` is the cost of painting house `1` with color `2`, and so on... Find the minimum cost to paint all houses.
Note:
All costs are positive integers.

**Example:**
```
Input: [[1,5,3],[2,9,4]]
Output: 5
Explanation: Paint house 0 into color 0, paint house 1 into color 2. Minimum cost: 1 + 4 = 5; 
             Or paint house 0 into color 2, paint house 1 into color 0. Minimum cost: 3 + 2 = 5. 
```

## My Solution
The same thought as the [previous question](https://www.lintcode.com/problem/515). But finding the minimum previous cost uses linear search, which is very slow...
```C++
    int minCostII(vector<vector<int>> &costs) {
        int n = costs.size();
        if(n < 1)
            return 0;

        int k = costs[0].size();
        vector<int> dp(costs[0]);

        for(int i = 1; i < n; i++) {
            vector<int> temp(dp);
            for(int j = 0; j < k; j++) {
                int min_prev = INT_MAX;
                for(int t = 0; t < k; t++) {
                    if(t != j && dp[t] < min_prev)
                        min_prev = dp[t];
                }
                temp[j] = min_prev + costs[i][j];
            }
            dp = temp;
        }
        
        return *min_element(dp.begin(), dp.end());
    }
```

## Classic Solution
Here's a much better solution to find the previous minimum cost. 

The key is to keep the most minimum cost `most_min` (and its index `most_min_index`) as well as the next minimum cost `next_min`. If the current color index `j` conflicts with the previous most minimum cost index `prev_most_min_index` (i.e. `j == prev_most_min_index`), we will add the `next_min`, otherwise, we'll add the `most_min` to the current color cost `cost[i][j]`.

During the traversal, we need to update the most minimum as well as the next minimum.

Here's the implementation.

```C++
    int minCostII(vector<vector<int>> &costs) {
        int n = costs.size();
        if(n < 1)
            return 0;
        int k = costs.front().size();

        vector<vector<int>> dp(costs);

        int most_min = 0;
        int next_min = 0;
        int most_min_index = -1;
        for(int i = 0; i < n; i++) {
            int prev_most_min = most_min;
            int prev_next_min = next_min;
            int prev_most_min_index = most_min_index;

            most_min = INT_MAX;
            next_min = INT_MAX;
            most_min_index = -1;
            for(int j = 0; j < k; j++) {
                dp[i][j] += (j == prev_most_min_index) ? prev_next_min : prev_most_min;

                if(dp[i][j] < most_min) {
                    next_min = most_min;
                    most_min = dp[i][j];
                    most_min_index = j;
                }
                else if(dp[i][j] < next_min) {
                    next_min = dp[i][j];
                }
            }
        }
        return most_min;
    }
```

**Optimization**
We actually don't need the whole 2D array, but the 3 variables `most_min`, `next_min` and `most_min_index` is already enough to calculate the current layer.

Here's the implementation.
```C++
    int minCostII(vector<vector<int>> &costs) {
        int n = costs.size();
        if(n < 1)
            return 0;
        int k = costs.front().size();

        int most_min = 0;
        int next_min = 0;
        int most_min_id = -1;
        for(int i = 0; i < n; i++) {
            int prev_most_min = most_min;
            int prev_next_min = next_min;
            int prev_most_min_id = most_min_id;

            most_min = INT_MAX;
            next_min = INT_MAX;
            most_min_id = -1;

            for(int j = 0; j < k; j++) {
                int curr_cost = costs[i][j] + (j == prev_most_min_id ? prev_next_min : prev_most_min);

                if(curr_cost < most_min) {
                    next_min = most_min;
                    most_min = curr_cost;
                    most_min_id = j;
                }
                else if(curr_cost < next_min) {
                    next_min = curr_cost;
                }
            }
        }
        return most_min;
    }
```