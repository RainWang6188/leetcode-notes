# 279. Perfect Squares

## Description
Given an integer `n`, return the least number of *perfect square* numbers that sum to `n`.

A **perfect square** is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, `1`, `4`, `9`, and `16` are perfect squares while `3` and `11` are not.

**Example 1:**
```
Input: n = 12
Output: 3
Explanation: 12 = 4 + 4 + 4.
```

## My Solution
This problem can be solved using DP. 

Suppose we want to know the minimum number of perfect square numbers that sum to `i`, and that of the previous `i-1` values have been computed and stored in `dp[]`. So we have

$$
    dp[i] = \min\big( dp[i-k] + 1 \big) \ \text{,where $k$ is all the possible perfect square numbers no larger than $i$}
$$

```C++
    int numSquares(int n) {
        vector<int> dp(n+1, INT_MAX);
        dp[0] = 0;
        
        for(int i = 1; i <= n; i++) {
            for(int k = 1; i - k * k >= 0; k++) {
                dp[i] = min(dp[i], dp[i-k*k] + 1);
            }
        }
        
        return dp.back();
    }
```

## Classic Solution - static DP
Recall the definition of **static local variable** in C/C++. 

Using the static keyword on a local variable changes its duration from automatic duration to static duration. This means the variable is now created at the start of the program, and destroyed at the end of the program (just like a global variable). As a result, the static variable will retain its value even after it goes out of scope!

As a result, when running the testcases, setting the `dp[]` as a `static vector` avoids redundant computation. Each time we only expands `dp[]` until it covers `n`, no need to recompute the previous values everytime calling this `numSquares()` function.

```C++
int numSquares(int n) {
    static vector<int> dp = {0};

    while(dp.size() <= n) {
        int m = dp.size();
        int square = INT_MAX;
        for(int k = 1; k * k <= m; k++) {
            square = min(square, dp[m-k*k] + 1);
        }
        dp.push_back(square);
    }
    
    return dp[n];
}
```
