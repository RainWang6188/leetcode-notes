# 213. House Robber Ⅱ

## Description
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged **in a circle**. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight **without alerting the police**.

**Example:**
```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```
## My Solution
The difference between this problem and the first version is that the house here is a circle, which means `house[0]` cannot be robbed with `house[n-1]` at the same time.

To avoid this condition, we can rob either `house[0] - house[n-2]` or `house[1] - house[n-1]` depending on which choice offers more money.

Then, this problem downgraded into version one with two passes.



```C++
int rob(vector<int>& nums) {
    int n = nums.size();
    if(n < 3)
        return n == 1 ? nums[0] : max(nums[0], nums[1]);
    
    vector<int> dp(n, 0);
    dp[0] = nums[0];
    dp[1] = max(nums[0], nums[1]);
    for(int i = 2; i < n - 1; i++)
        dp[i] = max(dp[i-2] + nums[i], dp[i-1]);
    int max_1 = dp[n-2];

    dp[1] = nums[1];
    dp[2] = max(nums[1], nums[2]);
    for(int i = 3; i < n; i++)
        dp[i] = max(dp[i-2] + nums[i], dp[i-1]);
    
    return max(dp[n-1], max_1);
}
```
## Classic Solution
Basically, the algorithm is almost the same:

1. Rob house `0 -> (n-2)`
2. Rob house `1 -> (n-1)`
3. Choose the bigger one as the answer