# 81. Search in Rotated Sorted Array II

## Description
There is an integer array `nums` sorted in non-decreasing order (**not** necessarily with distinct values).

Before being passed to your function, `nums` is rotated at an unknown pivot index `k` `(0 <= k < nums.length)` such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (`0`-indexed). For example, `[0,1,2,4,4,4,5,6,6,7]` might be rotated at pivot index `5` and become `[4,5,6,6,7,0,1,2,4,4]`.

Given the array `nums` after the rotation and an integer `target`, return `true` if `target` is in `nums`, or `false` if it is not in `nums`.

You must decrease the overall operation steps as much as possible.
## My Solution  
The only difference here is duplicates are allowed in the `nums` compared to the previous question [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/).

So we choose to skip all the duplicates at both ends. Other parts are the same as the original solution to the previous question.

```C++
bool search(vector<int>& nums, int target) {
    int low = 0;
    int high = nums.size() - 1;
    while(low < high) {
        if(nums[low] == nums[high]) {
            while(low < high && nums[low] == nums[low+1])
                low++;
            while(low < high && nums[high] == nums[high-1])
                high--;
        }
        int mid = low + ((high - low) >> 1);
        if(nums[mid] > nums[high])
            low = mid + 1;
        else
            high = mid;
    }
    
    int pivot = low;
    low = 0;
    high = nums.size() - 1;
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        int realMid = (mid + pivot) % nums.size();
        if(nums[realMid] >= target)
            high = mid;
        else
            low = mid + 1;
    }
    int realLow = (low + pivot) % nums.size();
    return nums[realLow] == target ? true : false;
}
```


## Classic Solution 
Here's an solution solves both problem with or without duplicates.

```C++
int search(vector<int>& nums, int target) {
    int low = 0;
    int high = nums.size() - 1;
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        if(nums[mid] == target)
            return mid;
        
        if(nums[mid] > nums[low]) {
            if(nums[low] <= target && target < nums[mid])
                high = mid - 1;
            else
                low = mid + 1;
        }
        else if(nums[mid] == nums[low])
            low++;
        else {
            if(nums[mid] < target && target <= nums[high])
                low = mid + 1;
            else
                high = mid - 1;
        }
    }
    return nums[low] == target ? low : -1;
}
```

## Best Solution for Interview
Since the best solution for [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) consists of two parts: finding the pivot index, and using binary search.

We can optimize the procedure of finding the pivot index when the array contains duplicates, according to the solution to [Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/). 

```C++
bool search(vector<int>& nums, int target) {
    int low = 0;
    int high = nums.size() - 1;
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        if(nums[mid] > nums[high])
            low = mid + 1;
        else if(nums[mid] < nums[high])
            high = mid;
        else {
            if(high > 0 && nums[high] < nums[high-1])
                low = high;
            else
                high--;
        }
    }
    
    int pivot = low;
    low = 0;
    high = nums.size() - 1;
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        int realMid = (mid + pivot) % nums.size();
        if(nums[realMid] < target)
            low = mid + 1;
        else
            high = mid;
    }
    int realLow = (low + pivot) % nums.size();
    return nums[realLow] == target ? true : false;
}
```