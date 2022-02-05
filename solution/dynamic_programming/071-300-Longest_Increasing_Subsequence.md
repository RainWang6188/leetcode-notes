# 300. Longest Increasing Subsequence

## Description
Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3,6,2,7]` is a subsequence of the array `[0,3,1,6,2,2,7]`.

**Example:**
```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```
 
## My Solution
This solution is a classic dynamic programming, where the state transition function can be easily derived as
```C++
    dp[i] = max(dp[k] + 1), where k < i && dp[k] < dp[i]
```
where `dp[i]` represents the longest increasing subsequence **ending at `i`**.

The code is as follows, whose time complexity is $O(N^2)$.

```C++
int lengthOfLIS(vector<int>& nums) {
    vector<int> dp(nums.size(), 1);
    int max_len = 1;
    
    for(int i = 1; i < nums.size(); i++) {
        for(int k = 0; k < i; k++) {
            if(nums[k] < nums[i])
                dp[i] = max(dp[i], dp[k] + 1);
        }
        max_len = max(max_len, dp[i]);
    }
    
    return max_len;
}
```

## Classic Solution - Patience Sorting
Actually, we can change the state a little bit to improve the time complexity.

`tails` is an array storing the smallest tail of all increasing subsequences with length `i+1` in `tails[i]`.
For example, say we have `nums = [4,5,6,3]`, then all the available increasing subsequences are:
```
len = 1   :      [4], [5], [6], [3]   => tails[0] = 3
len = 2   :      [4, 5], [5, 6]       => tails[1] = 5
len = 3   :      [4, 5, 6]            => tails[2] = 6
```
We can easily prove that `tails` is an increasing array. Therefore it is possible to do a binary search in `tails` array to find the one needs update (i.e. finding the leftmost `tails[index]` with `val > tails[index]` and update `tails[index+1] = val`)

Each time we only do one of the two:

(1) if x is larger than all `tails[i]`, append it, increase the size by 1.

(2) if `tails[i-1] < x <= tails[i]`, update `tails[i]`.

Doing so will maintain the tails invariant. The the final answer is just the size.

```C++
int lengthOfLIS(vector<int>& nums) {
    vector<int> dp(nums.size(), 0);
    int max_len = 0;
    
    for(auto val : nums) {
        int left = 0, right = max_len;
        while(left < right) {
            int mid = left + ((right - left) >> 1);
            if(dp[mid] < val)
                left = mid + 1;
            else
                right = mid;
        }
        
        dp[right] = val;
        
        if(right == max_len)
            max_len++;
    }
    
    return max_len;
}
```