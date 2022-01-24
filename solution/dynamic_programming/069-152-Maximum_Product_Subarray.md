# 152. Maximum Product Subarray

## Description
Given an integer array `nums`, find a contiguous non-empty subarray within the array that has the largest product, and return the product.

The test cases are generated so that the answer will fit in a **32-bit** integer.

A **subarray** is a **contiguous** subsequence of the array.

**Example:**
```
Input: nums = [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```
 
## Classic Solution
The hardest point of this question is that a previous negative product could be the positive product after multipling a negative value.

Basically, the problem is similar to Maximum Subarray problem, but for the two states we need to maintain:  `max_product` as well as the `min_product`, since `min_product` could be the maximum if the current value is negative.

The state transition function could be:
```
dp[i] = max(nums[i], maxProduct[i-1] * nums[i], minProduct[i-1] * nums[i])
```
Since `dp[i]` only depends on `max/minProduct[i-1]` we don't need to maintain the whole array but only the previous maximum/minimum product.

```C++
int maxProduct(vector<int>& nums) {
    int max_product = nums[0];
    int min_product = nums[0];
    int res = nums[0];
    
    for(int i = 1; i < nums.size(); i++) {
        int prev_max = max_product;
        int prev_min = min_product;

        max_product = max({nums[i], nums[i] * prev_max, nums[i] * prev_min});
        min_product = min({nums[i], nums[i] * prev_min, nums[i] * prev_max});
        
        res = max(res, max_product);
    }
    return res;
}
```
The code can also be modified as follows:
```C++
int maxProduct(vector<int>& nums) {
    int max_product = nums[0];
    int min_product = nums[0];
    int res = nums[0];
    
    for(int i = 1; i < nums.size(); i++) {
        if(nums[i] < 0)
            swap(max_product, min_product);
        
        max_product = max(nums[i], nums[i] * max_product);
        min_product = min(nums[i], nums[i] * min_product);
        
        res = max(res, max_product);
    }
    return res;
}```
