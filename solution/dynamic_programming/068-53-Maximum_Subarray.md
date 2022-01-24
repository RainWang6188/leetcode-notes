# 53. Maximum Subarray

## Description
Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

A **subarray** is a **contiguous** part of an array.

**Example:**
```
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```
## My Solution - Dynamic Programming
At first, I thought about the state of this program might be `dp[i, j]` which denotes the value of maximum subarray from index `i` to `j` in the `nums[]`. However, I cannot derive the state trasition function...

So, I tried to set the state as `dp[i]`, which denotes the value of maximum subarray ending at `nums[i]`. Then the state transition problem can be easily derived as 
```C++
    dp[i] = (dp[i-1] > 0 ? dp[i-1] : 0) + nums[i]
```
Here's the code.
```C++
int maxSubArray(vector<int>& nums) {
    vector<int> dp(nums.size(), 0);
    dp[0] = nums[0];
    int max_sum = dp[0];
    
    for(int i = 1; i < nums.size(); i++) {
        dp[i] = max(dp[i-1], 0) + nums[i];
        max_sum = max(max_sum, dp[i]);    
    }
    return max_sum;
}
```
Actually, `dp[i]` only depends on `dp[i-1]`. So we just need to record the previous dp value instead of maintaining the whole dp vector. And this is the famous Kadane's Algorithm.

## Classic Solution - Divide and Conquer
This problem can be solved using divide and conquer.

The maximum subarray lies in one of the following three cases:

1. left part
2. right part
3. middle part (a subarray containing the middle elements)

```C++
int maxSubArray(vector<int>& nums) {
    return maxSubArray(nums, 0, nums.size() - 1);
}

int maxSubArray(vector<int>& nums, int left, int right) {
    if(left > right)
        return INT_MIN;
    
    int mid = left + ((right - left) >> 1);
    int max_left = 0, cur_left = 0;
    int max_right = 0, cur_right = 0;
    for(int i = mid - 1; i >= left; i--) {
        cur_left += nums[i];
        max_left = max(max_left, cur_left);
    }
    for(int j = mid + 1; j <= right; j++) {
        cur_right += nums[j];
        max_right = max(max_right, cur_right);
    }
    
    return max({maxSubArray(nums, left, mid - 1), maxSubArray(nums, mid + 1, right), max_left + nums[mid] + max_right});
}
```