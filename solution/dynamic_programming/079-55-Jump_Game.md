# 55. Jump Game

## Description
You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return true if you can reach the last index, or false otherwise.

**Example:**
```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```


## My Solution
The state transition function is easy to derive.

`dp[i]` is true if and only if for some `j in [0,i)`, `dp[j] && j + nums[j] >= i`.

So the code is as follows:

```C++
bool canJump(vector<int>& nums) {
    vector<bool> dp(nums.size(), false);
    dp[0] = true;
    
    for(int i = 1; i < nums.size(); i++) {
        for(int j = i - 1; j >= 0; j--)
            if(dp[j] && j + nums[j] >= i) {
                dp[i] = true;
                break;
            }
    }
    return dp[nums.size()-1];
}
```
## Classic Solution
### Greedy
Always record the maximum index it can reach at each iteration.

```C++
bool canJump(vector<int>& nums) {
    int reach = 0;
    for(int i = 0; i < nums.size(); i++) {
        if(reach < i)
            return false;
        reach = max(reach, nums[i] + i);
    }
    return true;
}
```
