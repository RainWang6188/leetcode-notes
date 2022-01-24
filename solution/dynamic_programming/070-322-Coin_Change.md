# 322. Coin Change

## Description
You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

**Example:**
```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```
## My Solution
The state transition function is not hard to come up with.

Suppose `dp[i]` represents the fewest fewest number of coins that you need to make up the amount `i`, then we have:

```
    dp[i] = min(dp[i - k] + 1), where k is the valid denominations
```
which means $k\in$ `coins` && $k\leq i$

```C++
int coinChange(vector<int>& coins, int amount) {
    int n = coins.size();
    vector<long> dp(amount + 1, INT_MAX);
    dp[0] = 0;
    
    for(int i = 1; i <= amount; i++) {
        for(int val : coins) {
            if(val <= i)
                dp[i] = min(dp[i], dp[i-val] + 1);
        }
    }
    return static_cast<int>(dp[amount]) == INT_MAX ? -1 : static_cast<int>(dp[amount]);
}
```

Note that we initialize `dp[]` with `INT_MAX` which may cause integer overflow in the for loop, so we change the type of `dp[]` into `long`. However, a better solution is initialize `dp[]` with `amount + 1`, since the number of coins won't exceed `amount`.

## Classic Solution

The above solution can be further optimized by exchaning the outer for loop with the inner loop to avoid unnecessary loops.

```C++
int coinChange(vector<int>& coins, int amount) {
    int n = coins.size();
    int MAX = amount + 1;
    vector<int> dp(amount + 1, MAX);
    dp[0] = 0;
    
    for(int val : coins) {
        for(int i = val; i <= amount; i++) 
            dp[i] = min(dp[i], dp[i-val] + 1);
    }
    return dp[amount] == MAX ? -1 : dp[amount];
}
```