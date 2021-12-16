# 41. First Missing Positive
## Description
Given an unsorted integer array nums, return the smallest missing positive integer.

You must implement an algorithm that runs in `O(n)` time and uses constant extra space.

 
**Example 1:**
```
Input: nums = [1,2,0]
Output: 3
```

**Example 2:**
```
Input: nums = [3,4,-1,1]
Output: 2
```

**Example 3:**
```
Input: nums = [7,8,9,11,12]
Output: 1
```

## My Solution
I didn't work out with the whole solution... But I got the idea that we need to use the original array `nums` to record whether an element is found. According to my first thought, once element `i` is found, `nums[i-1]` should be set to `1`, otherwise it should be set to `0`. However, this solution cannot satisfy the `O(1)` space.

The key is that we can simply use `nums[nums[i]-1] = nums[i]` to record the element (i.e. if we find `2`, we set `nums[1] = 2`), so no extra space is needed. To satisfy the constraint of time complexity, we can keep swapping `nums[i]` with `nums[nums[i]-1]` until the the element on index `i` is equal to `i+1`.

Here's the code:

``` C++
int firstMissingPositive(vector<int>& nums) {
    for(int i = 0; i < nums.size(); i++){
        while(nums[i] > 0 && nums[i] <= nums.size() && nums[nums[i] - 1] != nums[i])
            swap(nums[i], nums[nums[i] - 1]);
    }
    
    int index;
    for(index = 0; index < nums.size(); index++)
        if(nums[index] != index+1)
            break;
    
    return index + 1;
}
```

## Classic Solution
We only need to keep track of which ones of the first n positive integers occur in the array. To do this with no extra space, we can treat the array like a boolean array, where `sign(arr[i])` indicates whether the $i^{th}$ positive integer is present or not. First, we replace all non-positive values with `n + 1`, since we'll only use these indices as storage space. Then, for every one of the first `n` positive numbers in the array, we turn the value at their corresponding index negative (i.e. `nums[i] < 0` indicates `i+1` is found). Finally, all we need to do is check for the first positive value, which will give us the first missing positive. If we find no such index, then all natural numbers up to `n` are present, so we return `n + 1`.

```C++
int firstMissingPositive(vector<int>& nums) {
    int n = nums.size();
    for(int i = 0; i < n; i++)
        if(nums[i] <= 0)
            nums[i] = n + 1;
    
    for(int i = 0; i < n; i++)
        if(abs(nums[i]) <= n && nums[abs(nums[i])-1] > 0)
            nums[abs(nums[i]) - 1] *= -1;
    
    for(int i = 0; i < n; i++)
        if(nums[i] > 0)
            break;

    return i + 1;
}
```
Note that we should not forget to use `abs()` when retrieving the value of `nums[abs(nums[i]) - 1]`.