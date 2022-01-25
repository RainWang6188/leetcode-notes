# 337. Combination Sum Ⅳ

## Description
Given an array of distinct integers `nums` and a target integer `target`, return the number of possible combinations that add up to `target`.

The test cases are generated so that the answer can fit in a **32-bit integer**.

**Example:**
```
Input: nums = [1,2,3], target = 4
Output: 7
Explanation:
The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
Note that different sequences are counted as different combinations.
```
## My Solution
The state transition function is not hard to derive.

Suppose `dp[i]` represents the number of combinations for sum `i`, then we have `dp[i] = Sum(dp[i-k])`, where `k` is the valid number in `nums`. 

```C++
int combinationSum4(vector<int>& nums, int target) {
    vector<unsigned int> dp(target + 1, 0);
    dp[0] = 1;

    for(int i = 1; i <= target; i++) {
        for(int num : nums) {
            if(num > i)
                continue;
            else
                dp[i] += dp[i-num];
        }
    }
    return dp[target];
}
```
## Classic Solution
The above solution can be optimized by first sorting the `nums` and then finding the valid `num` would take less time (to break)

```C++
int combinationSum4(vector<int>& nums, int target) {
    sort(nums.begin(), nums.end());
    vector<unsigned int> dp(target + 1, 0);
    dp[0] = 1;
    
    for(int i = 1; i <= target; i++) {
        for(int num : nums) {
            if(num > i)
                break;
            else
                dp[i] += dp[i-num];
        }
    }
    return dp[target];
}
```