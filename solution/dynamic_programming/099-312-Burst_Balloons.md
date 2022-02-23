# 312. Burst Balloons

## Description
You are given `n` balloons, indexed from `0` to `n - 1`. Each balloon is painted with a number on it represented by an array `nums`. You are asked to burst all the balloons.

If you burst the `ith` balloon, you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. If `i - 1` or `i + 1` goes out of bounds of the array, then treat it as if there is a balloon with a `1` painted on it.

Return the maximum coins you can collect by bursting the balloons wisely.

**Example 1:**
```
Input: nums = [3,1,5,8]
Output: 167
Explanation:
nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
coins =  3*1*5    +   3*5*8   +  1*3*8  + 1*8*1 = 167
```

## Classic Solution
>excperted from [here](https://leetcode.com/problems/burst-balloons/discuss/76229/For-anyone-that-is-still-confused-after-reading-all-kinds-of-explanations...)

First of all, `dp[i][j]` in here means, the maximum coins we get after we burst all the balloons between `i` and `j` in the original array.

For example with input `[3,1,5,8]`:

`dp[0][0]` means we burst ballons between `[0,0]`, which means we only burst the first balloon in original array. So `dp[0][0]` is 1 * 3 * 1 = 3.

`dp[1][1]` means we burst balloons between `[1,1]`, which means we only burst the second ballon in the original array. So `dp[1][1]` is 3 * 1 * 5 = 15.

So in the end for this problem we want to find out `dp[0][ arr.length - 1 ]`, which is the maximum value we can get when we burst all the balloon between `[0 , length -1]`

To get that we need the transition function:
```C++
for (int k = left; k <= right; ++k)
    dp[left][right] = max(dp[left][right], nums[left-1] * nums[k] * nums[right+1] + dp[left][k-1] + dp[k+1][right])

```

This transition function basically says in order to get the maximum value we can get for bursting all the balloons between `[i, j]` , we just loop through each balloon between these two indexes and make them to be the last balloon to be burst,

why we pick it as the last balloon to burst ?

For example when calculating `dp[0,3]` and picking index `2` as the last balloon to burst, `[ 3 , 1 , 5 , 8]` , that means `5` is the last balloon to burst between `[0,3]` , to get the maximum value when picking `5` as the last balloon to burst :

**max = maximum value of bursting all the balloon on the left side of 5 + maximum value of bursting all the balloon on the right side of 5 + bursting balloon 5 when left side and right side are gone**.

That is `dp[0, 1] + nums[0 - 1] * nums[2] * nums[3 + 1] + + dp[3,3]`;

That is `dp[left, k - 1] + nums[left - 1] * nums[k] * nums[right + 1] + dp[k+1, right]` ;

to get the maximum `dp[left, right]` we just loop through all the possible value of `k` to get the maximum.

```C++
int maxCoins(vector<int>& nums) {
    int n = nums.size();
    nums.insert(nums.begin(), 1);
    nums.insert(nums.end(), 1);
    
    vector<vector<int>> dp(nums.size(), vector<int>(nums.size(), 0));
    
    for(int j = 1; j <= n; j++) {
        for(int i = j; i >= 1; i--) {
            int globalCoins = 0;
            for(int k = i; k <= j; k++) {
                int localCoins = dp[i][k-1] + dp[k+1][j] + nums[i-1] * nums[k] * nums[j+1];
                globalCoins = max(globalCoins, localCoins);
            }
            dp[i][j] = globalCoins;
        }
    }
    
    return dp[1][n];
}
```