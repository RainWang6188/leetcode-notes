# 88. Merge Sorted Array

## Description

You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing** order, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

Merge `nums1` and `nums2` into a single array sorted in **non-decreasing** order.

The final sorted array should not be returned by the function, but instead be stored inside the array `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

**Example:**
```
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.
```

## Classic Solution

[Here](https://leetcode.cn/problems/merge-sorted-array/solution/88-by-ikaruga/)'s a very clear demonstration of how to solve this question.

Basically, it is better to place the items from the end of the array.

We maintain 3 pointers:
- `m` denotes the end of the elements of `nums1` to be merged
- `n` denotes the end of the elements of `nums2` to be merged
- `index` denotes the end of the `0` which should be replaced during the merge in the `nums1`.

We compare the last element of `nums1` and `nums2`:
- `nums1[m] > nums2[n]`: replace the last `0` in `nums1` with the last element of `nums1` (i.e. swapping `nums1[m]` with `nums1[index]`)
- `nums1[m] <= nums2[n]`: replace the last `0` in `nums1` with the last element of `nums2` (i.e. swapping `nums1[index]` with `nums2[n]`)

```C++
void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
    int index = m + n - 1;
    m--;
    n--;
    
    while(n >= 0) {
        while(m >= 0 && nums1[m] > nums2[n])
            swap(nums1[m--], nums1[index--]);
        
        swap(nums1[index--], nums2[n--]);
    }
}
```