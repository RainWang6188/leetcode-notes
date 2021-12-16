# 80. Remove Duplicates from Sorted Array II
## Description
Given an integer array nums sorted in non-decreasing order, remove some duplicates in-place such that each unique element appears at most twice. The relative order of the elements should be kept the same.

**Example**
```
Input: nums = [0,0,1,1,1,1,2,3,3]
Output: 7, nums = [0,0,1,1,2,3,3,_,_]
Explanation: Your function should return k = 7, with the first seven elements of nums being 0, 0, 1, 1, 2, 3 and 3 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

## My Solution
The algorithm is basically similar to [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/). The only difference is that we need to record the number of occurrence of duplicates (as `count`), only increment the `index` when the `count` is 1 or 2, so that we will save the unique elements at most twice.
The edge case of `nums` is emtpy should be taken care of.  

``` C++
int removeDuplicates(vector<int>& nums) {
    if(nums.size() == 0)
    return 0;

    int index = 0, count = 1;
    for(int i = 1; i < nums.size(); i++){
        if(nums[i] == nums[index]){
            count++;
            if(count == 1 || count == 2)
                nums[++index] = nums[i];
        }
        else {
            count = 1;
            nums[++index] = nums[i];
        }
    }
    
    return index + 1;
}
```

## Classic Solution
Still a two pointer solution. `i` always points to the next position of the rear of the valid array (i.e. the number of elements in valid array). There're two conditions when we can add an element to the valid array:
    1) the first two elements can always be added, whether they're equal or not (do not exceed two elements)
    2) Since `i` points to the `rear+1` position, `i-2` points to the `rear-1` position. So we only need to compare the current element with `nums[i-2]`, if they're not equal, we can simply add it to the valid array (because it won't exceed two elements).

```C++
int removeDuplicates(vector<int>& nums) {
    int i = 0;
    for (int n : nums)
        if (i < 2 || n > nums[i-2])
            nums[i++] = n;
    return i;
}
```
We can be easily generalized to "at most K duplicates":
```C++
int removeDuplicates(vector<int>& nums, int k) {
    int i = 0;
    for (int n : nums)
        if (i < k || n > nums[i-k])
            nums[i++] = n;
    return i;
}
```