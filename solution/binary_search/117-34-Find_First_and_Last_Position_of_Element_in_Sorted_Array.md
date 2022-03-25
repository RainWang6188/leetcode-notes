# 34. Find First and Last Position of Element in Sorted Array

## Description
Given an array of integers `nums` sorted in **non-decreasing** order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example:**
```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

## My Solution
Obviously, we should use binary search to find the indices.

Searching for the index of first occurrence using binary search is easy, so we can modify the same algorithm a bit to find the last occurrence by the following steps:

- Reverse the original array, now that the last index becomes the first index, which is easy to find. Note that here the array is **non-increasing** instead of **non-descreasing**.

- Suppose the first index returned from the previous step is `id`, then the last occurrence in the original array is `n-1-id`.

```C++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        if(nums.empty())
            return {-1, -1};
        int firstIndex = findFirstIndex(nums, target, true);
        
        if(firstIndex == -1)
            return {-1, -1};
        
        reverse(nums.begin(), nums.end());
        int lastIndex = nums.size() - 1 - findFirstIndex(nums, target, false);
        return {firstIndex, lastIndex};
    }
    
private:
    int findFirstIndex(vector<int>& nums, int target, bool isIncreasing) {
        int low = 0;
        int high = nums.size() - 1;
        while(low < high) {
            int mid = low + ((high - low) >> 1);
            if(isIncreasing) {
                if(nums[mid] < target)
                    low = mid + 1;
                else
                    high = mid;
            }
            else {
                if(nums[mid] <= target)
                    high = mid;
                else
                    low = mid + 1;
            }
        }
        return (nums[low] == target ? low : -1);
    }
};
```

But `std::reverse()` is $O(N)$ time complexity, so it doesn't satisfy the requirement.

## Classic Solution
The problem can be simply broken down as two binary searches for the begining and end of the range, respectively:
### 1. Finding left boundary
First let's find the left boundary of the range. We initialize the range to `[i=0, j=n-1]`. In each step, calculate the middle element `[mid = (i+j)/2]`. Now according to the relative value of `A[mid]` to target, there are three possibilities:

1. If `A[mid] < target`, then the range must begins on the right of mid (hence `i = mid+1` for the next iteration)
2. If `A[mid] > target`, it means the range must begins on the left of mid (`j = mid-1`)
3. If `A[mid] = target`, then the range must begins on the left of or at mid (`j= mid`)

Since we would move the search range to the same side for case 2 and 3, we might as well merge them as one single case so that less code is needed: If `A[mid] >= target`, then `j = mid`;

### 2. Finding the right boundary
For the right of the range, we can use a similar idea. Again we can come up with several rules:

- If `A[mid] > target`, then the range must begins on the left of mid (`j = mid-1`)
- If `A[mid] < target`, then the range must begins on the right of mid (hence `i = mid+1` for the next iteration)
- If `A[mid] = target`, then the range must begins on the right of or at mid (`i= mid`)

Again, we can merge condition 2 and 3 into: If `A[mid] <= target`, then `i = mid`;

However, if we still use the same way `mid=(left+right)/2` to find the middle element, the terminal conditions no longer works, considering cases like `A=[5 7], target = 5`.

That is becase `mid=(left+right)/2` makes the `mid` is rounded to the lowest integer. In other words, `mid` is always biased towards the `left`. To keep the search range moving, we can use `mid=(left+right+1)/2` (a more elegant way is `mid=high-((high-low)>>1)`), which results in the `mid` biased to the right.

```C++
vector<int> searchRange(vector<int>& nums, int target) {
    vector<int> res = {-1, -1};
    if(nums.empty())
        return res;
    
    int low = 0;
    int high = nums.size() - 1;
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        if(nums[mid] < target)
            low = mid + 1;
        else
            high = mid;
    }
    if(nums[low] != target)
        return res;
    else
        res[0] = low;
    
    high = nums.size() - 1;
    while(low < high) {
        int mid = high - ((high - low) >> 1);
        if(nums[mid] > target)
            high = mid - 1;
        else
            low = mid;
    }
    res[1] = high;
    return res;
}
```

Another trick here is that we don't need to set `low = 0` after the first binary search, since the right boundary must on the right of the left boundary.