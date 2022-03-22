# 33. Search in Rotated Sorted Array

## Description
There is an integer array `nums` sorted in ascending order (with distinct values).

Prior to being passed to your function, `nums` is possibly rotated at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` after the possible rotation and an integer target, return the index of target if it is in `nums`, or `-1` if it is not in `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

**example**
```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

## My Solution
### 1. Linear Search
My solution is to use linear search.
```C++
int search(vector<int>& nums, int target) {
    for(int i = 0; i < nums.size(); i++)
        if(nums[i] == target)
            return i;
    
    return -1;
}
```

### 2. Binary Search
Another implementation is to use binary search, which satisfies the constraint of $O(\log N)$ time complexity.

It is known that binary search can only be used when the array is sorted. As a result, we must only implement binary search in the interval where there's no rotation pivot. 

To do that, we first compare `nums[mid]` with `nums[low]` to know if the left part is ascending or not. If so, we check if the `target` is in the left part by checking if `nums[low] <= target <= nums[mid]`; otherwise, we search in the right part.
So is to the circumstances where right part is ascending.

```C++
int search(vector<int>& nums, int target) {
    int low = 0;
    int high = nums.size() - 1;
    
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        if(nums[mid] >= nums[low]) {
            if(nums[mid] >= target && nums[low] <= target )
                high = mid;
            else
                low = mid + 1;
        }
        else {
            if(nums[mid] <= target && nums[high] >= target)
                low = mid;
            else
                high = mid;
        }
    }
    return (nums[low] == target) ? low : -1;
}
```

## Classic Solution
### Solution 1
A classic solution is provide by [1337beef](https://leetcode.com/problems/search-in-rotated-sorted-array/discuss/14425/Concise-O(log-N)-Binary-search-solution). He first finds the rotation pivot (i.e. the smallest element), and then implement the binary search by remapping the current array to an ordinary ascending array.

```C++
int search(vector<int>& nums, int target) {
    int n = nums.size();
    int low = 0;
    int high = nums.size() - 1;
    int pivot = 0;
    
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        if(nums[mid] > nums[high])
            low = mid + 1;
        else
            high = mid;
    }
    
    pivot = low;
    low = 0;
    high = nums.size() - 1;
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        int realmid = (pivot + mid) % n;
        if(nums[realmid] >= target) 
            high = mid;
        else
            low = mid + 1;
    }
    int reallow = (low + pivot) % n;
    return (nums[reallow] == target) ? reallow : -1;        
}
```

For those who struggled to figure out why it is `realmid=(mid+pivot)%n`: you can think of it this way:

If we want to find `realmid` for array `[4,5,6,7,0,1,2]`, you can shift the part before the rotating point to the end of the array (after 2) so that the sorted array is "recovered" from the rotating point but only longer, like this: `[4, 5, 6, 7, 0, 1, 2, 4, 5, 6, 7]`. The real mid in this longer array is the middle point between the rotating point and the last element: `(pivot + (hi+pivot)) / 2`. `(hi + pivot)` is the index of the last element. And of course, this result is larger than the real middle. So you just need to wrap around and get the remainder: `((pivot + (hi + pivot)) / 2) % n`. And this expression is effectively `(pivot + hi/2) % n`, which is `(pivot+mid) % n`.

### Solution 2
Another elegant solution which avoids finding the pivot.
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