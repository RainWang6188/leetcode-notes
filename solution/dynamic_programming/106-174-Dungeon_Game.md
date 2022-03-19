# 174. Dungeon Game

## Description
The demons had captured the princess and imprisoned her in the bottom-right corner of a `dungeon`. The `dungeon` consists of `m x n` rooms laid out in a 2D grid. Our valiant knight was initially positioned in the **top-left** room and must fight his way through dungeon to rescue the princess.

The knight has an initial health point represented by a positive integer. If at any point his health point drops to `0` or below, he dies immediately.

Some of the rooms are guarded by demons (represented by negative integers), so the knight loses health upon entering these rooms; other rooms are either empty (represented as `0`) or contain magic orbs that increase the knight's health (represented by positive integers).

To reach the princess as quickly as possible, the knight decides to move only **rightward** or **downward** in each step.

Return the knight's minimum initial health so that he can rescue the princess.

Note that any room can contain threats or power-ups, even the first room the knight enters and the bottom-right room where the princess is imprisoned.

**Example:**
```
Input: dungeon = [[-2,-3,3],[-5,-10,1],[10,30,-5]]
Output: 7
Explanation: The initial health of the knight must be at least 7 if he follows the optimal path: RIGHT-> RIGHT -> DOWN -> DOWN.
```

## My Solution
This question can be solved using dynamic programming.

Suppose `dp[i][j]` represents the minimum health required for the knight at position `(i, j)`, then we have the state transition function as follows:
```C++
dp[i][j] = max(1, min(dp[i+1][j], dp[i][j+1]) - dungeon[i][j]);
```

Then we start traversal the dungeon from the bottom right to the top left, where `dp[0][0]` would be the minimum required heath to rescue the princess sucessfully. 

```C++
int calculateMinimumHP(vector<vector<int>>& dungeon) {
    if(dungeon.empty())
        return 1;
    
    int maxHealth = 1;
    int m = dungeon.size();
    int n = dungeon.front().size();
    
    vector<vector<int>> dp(m+1, vector<int>(n+1, INT_MAX));
    dp[m][n-1] = 1;
    dp[m-1][n] = 1;
    
    for(int i = m-1; i >= 0; i--) {
        for(int j = n-1; j >= 0; j--) {
            dp[i][j] = max(1, min(dp[i+1][j], dp[i][j+1]) - dungeon[i][j]);
        }
    }
    
    return dp[0][0];
}
```
**Optimization**
Instead of using a 2D matrix, we can use only a 1D matrix, for `dp[i][j]` only depends on the right and below adjacent element.

```C++
int calculateMinimumHP(vector<vector<int>>& dungeon) {
    if(dungeon.empty())
        return 1;
    
    int m = dungeon.size();
    int n = dungeon.front().size();
    
    vector<int> dp(n+1, INT_MAX);
    dp[n] = INT_MAX;
    dp[n-1] = 1;
    
    for(int i = m-1; i >= 0; i--) {
        for(int j = n-1; j >= 0; j--) {
            dp[j] = max(1, min(dp[j], dp[j+1]) - dungeon[i][j]);
        }
    }
    
    return dp[0];
}
```