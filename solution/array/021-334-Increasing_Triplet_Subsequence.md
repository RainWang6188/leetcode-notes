# 334. Increasing Triplet Subsequence
## Decription
Given an integer array `nums`, return `true` if there exists a triple of indices `(i, j, k)` such that `i < j < k` and `nums[i] < nums[j] < nums[k]`. If no such indices exists, return `false`.

**example**
```
Input: nums = [2,1,5,0,4,6]
Output: true
Explanation: The triplet (3, 4, 5) is valid because nums[3] == 0 < nums[4] == 4 < nums[5] == 6.
```

## My Solution
I didn't figure this question out by myself...

## Classic Solution
A really simple yet tricky solution is as follows. It employs greedy algorithm and uses two variables `c1` and `c2` to record the candidate for the first two element of the increasing triplet subsequence. The code is as follows:
```C++
bool increasingTriplet(vector<int>& nums) {
    int c1 = INT_MAX, c2 = INT_MAX;
    
    for(int i = 0; i < nums.size(); i++) {
        if(nums[i] <= c1) 
            c1 = nums[i];
        else if(nums[i] <= c2)
            c2 = nums[i];
        else
            return true;
    }
    return false;
}
```

At first, I thought about this method by myself, but I thought it won't work with testcase like `[3,4,1,5]`, since `c1 = 1, c2 = 4` when scanning to the thrid element. But when I take a closer look, I found out that `c1` and `c2` are just candidate, the size of the longest increasing subsequence is recorded (since `c1` and `c2` have value).

Actually, this is a simplified version of the solution to [Longest Inscreasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/) question. The algorithm uses an extra vector to record the candidates of the LIS and would find the fittest place in the extra vector by `lower_bound()` function. This solution is as follows:
```C++
int lengthOfLIS(vector<int>& nums) {
    vector<int> seq;
    
    for(int i = 0; i < nums.size(); i++) {
        int val = nums[i];
        auto it = lower_bound(seq.begin(), seq.end(), val);
        
        if(it == seq.end()) 
            seq.push_back(val);
        else {
            if(*it > val) 
                *it = val;
        }
    }
    return seq.size();
}
```
And the previous solution to the triplet subsequence just replaces the `lower_bound()` by `if-else` sentence and uses two variables to store the sequence instead of an extra vector.