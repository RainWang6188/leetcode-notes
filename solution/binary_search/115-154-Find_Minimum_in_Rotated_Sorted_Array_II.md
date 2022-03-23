# 154. Find Minimum in Rotated Sorted Array II

## Description
Suppose an array of length `n` sorted in **ascending** order is rotated between `1` and `n` times. For example, the array `nums = [0,1,4,4,5,6,7]` might become:

- `[4,5,6,7,0,1,4]` if it was rotated `4` times.
- `[0,1,4,4,5,6,7]` if it was rotated `7` times.

Notice that rotating an array `[a[0], a[1], a[2], ..., a[n-1]]` `1` time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` that may contain **duplicates**, return the minimum element of this array.

You must decrease the overall operation steps as much as possible.

**Example:**
```
Input: nums = [2,2,2,0,1]
Output: 0
```

## My Solution
I skip the duplicates at both ends before each iteration in the binary search.

**Note:**
- After the skipping procedure, we need to check if the invariant `low < high` still holds.
- `nums[mid] <= nums[high]` and `high = mid` should be combined. Be careful about the equal sign.

```C++
int findMin(vector<int>& nums) {
    int low = 0;
    int high = nums.size() - 1;
    while(low < high) {
        if(nums[low] == nums[high]) {
            while(low < high && nums[low] == nums[low+1])
                low++;
            while(low < high && nums[high] == nums[high-1])
                high--;
        }
        if(low >= high)
            break;
        
        int mid = low + ((high - low) >> 1);
        if(nums[mid] <= nums[high])
            high = mid;
        else
            low = mid + 1;
    }
    return nums[low];
}
```

## Classic Solution
When `num[mid] == num[hi]`, we couldn't sure the position of minimum in mid's left or right, so just let upper bound reduce one.

But to find the minimum element **and its index**, we need to make sure the pivot index will be not passed by at `high--`, so we have to add a judgement beforehand.

```C++
int findMin(vector<int>& nums) {
    int low = 0;
    int high = nums.size() - 1;
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        if(nums[mid] > nums[high])
            low = mid + 1;
        else if(nums[mid] < nums[high])
            high = mid;
        else {
            if(high > 0 && nums[high] < nums[high-1]) {
                low = high;
            }
            else
                high--;
        }
    }
    return nums[low];
}
```