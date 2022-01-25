# 198. House Robber
## Description
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight **without alerting the police**.

**Example:**
```
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```

## My Solution
After so many previous pratices, I think the state `dp[i]` would be the maximum amount of money I can get after robbing some houses in $[0, i]$, where house `i` must be robbed.

At first, I thought if I rob house `i`, then house `i-1` mustn't be robbed, and house `i-2` should be robbed. So the state transition function would be `dp[i] = dp[i-2] + nums[i]`.

However, the above state transition function is false, considering the input `[2,1,1,2]`. Actually, when house `i` must be robbed, house `i-2` is not must, but need to compare with house `i-3`.

The final state transition function is as follows:
```C++
    dp[i] = max(dp[i-2], dp[i-3]) + nums[i]
```
The C++ code is as follows:
```C++
int rob(vector<int>& nums) {
    if(nums.size() < 3)
        return nums.size() == 1 ? nums[0] : max(nums[0], nums[1]);
    
    vector<int> dp(nums.size(), 0);
    dp[0] = nums[0];
    dp[1] = nums[1];
    dp[2] = nums[0] + nums[2];
    int max_amount = max({dp[0], dp[1], dp[2]});
    
    for(int i = 3; i < nums.size(); i++) {
        dp[i] = max(dp[i-2], dp[i-3]) + nums[i];
        max_amount = max(max_amount, dp[i]);
    }
    
    return max_amount;
}
```
## Classic Solution
A much more intuitive state transition function derived as follows:

A robber has 2 options: a) rob current house `i`; b) don't rob current house.

If an option "a" is selected it means she can't rob previous `i-1` house but can safely proceed to the one before previous `i-2` and gets all cumulative loot that follows.

If an option "b" is selected the robber gets all the possible loot from robbery of `i-1` and all the following buildings.

So the state transition function is:
```C++
    dp[i] = max(dp[i-2] + nums[i], dp[i-1])
```

```C++
int rob(vector<int>& nums) {
    if(nums.size() < 2)
        return nums[0];
    
    vector<int> dp(nums.size(), 0);
    dp[0] = nums[0];
    dp[1] = max(nums[0], nums[1]);
    
    for(int i = 2; i < nums.size(); i++) {
        dp[i] = max(dp[i-2] + nums[i], dp[i-1]);
    }
    return dp[nums.size()-1];
}
```