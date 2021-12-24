# 164. Maximum Gap

## Description
Given an integer array `nums`, return the maximum difference between two successive elements in its sorted form. If the array contains less than two elements, return `0`.

You must write an algorithm that runs in linear time and uses linear extra space.

**example**
```
Input: nums = [3,6,9,1]
Output: 3
Explanation: The sorted form of the array is [1,3,6,9], either (3,6) or (6,9) has the maximum difference 3.
```

## My Solution
My solution does not satisfy the requirement of time complexity exactly. I simply use the `sort` and linear search for the maximum gap.

```c++
int maximumGap(vector<int>& nums) {
    int max_gap = 0;
    sort(nums.begin(), nums.end());
    for(int i = 0; i < nums.size() - 1; i++) {
        max_gap = max<int>(max_gap, nums[i + 1] - nums[i]);
    }
    
    return max_gap;
}
```

## Classic Solution
The recommended solution uses the idea of bucket sort. 

First, we find the largest and the smallest element of `nums` array and compute the `gap`. Since the difference between any two elements of `nums` won't exceed that `gap`, we can divide the `nums` into buckets with the size of `max(1, gap / (n-1))`, denoted as `bucket_size`. (**Note that the maximum gap cannot be smaller than the `bucket_size`, so any gaps within the same bucket is not the amount we are looking for.**) As a result, we have at most `gap / bucket_size + 1` buckets.

So we only need to find the largest gap between the buckets (i.e. the gap between the maximum element of the current bucket and the minimum element of the next bucket).

The code is as follows:
```C++
int maximumGap(vector<int>& nums) {
    int n = nums.size();
    if(n < 2)
        return 0;
    
    auto lu = minmax_element(nums.begin(), nums.end());
    int l = *lu.first, u = *lu.second;
    
    int bucket_size = max((u - l) / (n - 1), 1);
    int bucket_num = (u - l) / bucket_size + 1;
    vector<int> bucket_min(bucket_num, INT_MAX);
    vector<int> bucket_max(bucket_num, INT_MIN);
    
    for(int num : nums) {
        int k = (num - l) / bucket_size;
        if(num < bucket_min[k])
            bucket_min[k] = num;
        if(num > bucket_max[k])
            bucket_max[k] = num;
    }
    
    int i = 0, j = 0;
    int max_gap = bucket_max[0] - bucket_min[0];
    while(1) {          
        for(j = i + 1; j < bucket_num && bucket_min[j] == INT_MAX; j++)
            ;
        if(j == bucket_num)
            break;
        
        max_gap = max(max_gap, bucket_min[j] - bucket_max[i]);
        i = j;
    }
    return max_gap;
}
```