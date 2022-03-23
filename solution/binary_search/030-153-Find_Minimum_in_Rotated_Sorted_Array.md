# 153. Find Minimum in Rotated Sorted Array

## Description
Suppose an array of length `n` sorted in ascending order is rotated between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:

- `[4,5,6,7,0,1,2]` if it was rotated 4 times.
- `[0,1,2,4,5,6,7]` if it was rotated 7 times.

Notice that rotating an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` of unique elements, return the minimum element of this array.

You must write an algorithm that runs in `O(log n)` time.

 
## My Solution
### 1. Linear Search
This solution doesn't satisfy the requirement of $O(\log N)$ time complexity. 
```c++
int findMin(vector<int>& nums) {
    int min_val = 5000;
    for(int num : nums) 
        min_val = min(min_val, num);
    return min_val;
}
```

### 2. Set
Another implementation is to employ `set` to store all the elements in `nums`. And the minimum element is the first one in the `set`.
```C++
int findMin(vector<int>& nums) {
    set<int> arr(nums.begin(), nums.end());
    set<int>::iterator it = arr.begin();
    return *it;
}
```

## Classic Solution
The classic solution is to use binary search.

Since the original array is sorted, finding the minimum element is to find the rotation point (i.e. the point where `a[i] < a[i-1]`). As a result, we can use binary search to find that point.

On every iteration, we compare `nums[mid]` with `nums[low]`:
 
- If `nums[mid] < nums[low]`, it means the rotation point is in the left part, so we set `high = mid`, continuing the search. Note that we set `high = mid` instead of `high = mid - 1` owing to the fact that `nums[mid]` can be the rotation point.

- If `nums[mid] >= nums[low]`, it means the rotation point is in the right part, so we set `low = mid + 1`, continuing the search. Here the `==` condition in the `if()` is to include the circumstance where `low == mid == high - 1`. 

```C++
int findMin(vector<int>& nums) {
    int low = 0;
    int high = nums.size() - 1;

    while(low < high) {
        if(nums[low] < nums[high])
            return nums[low];

        int mid = low + ((high - low) >> 1);
        if(nums[mid] >= nums[low])
            low = mid + 1;
        else
            high = mid;
    }
    return nums[low];
}
```

**Optimization:**
Here's an version no needs to check the ending condition of the loop:
```C++
int findMin(vector<int>& nums) {
    int low = 0;
    int high = nums.size() -1;
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        if(nums[mid] <= nums[high])
            high = mid;
        else
            low = mid + 1;
    }
    return nums[low];
}
```