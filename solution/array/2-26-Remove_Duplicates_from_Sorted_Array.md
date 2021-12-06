# 2-26-Remove_Duplicates_from_Sorted_Array
## 1. Description
Given an integer array `nums` sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. If there are `k` elements after removing the duplicates, then the first `k` elements of nums should hold the final result. It does not matter what you leave beyond the first `k` elements.

**example**
```
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

## 2. My Solution
The algorithm is similar to [the Remove Element problem](https://leetcode.com/problems/remove-element/). We use two pointers, `i` to traverse the array, and **`index` to indicate the rear of valid (no duplicate element) array**. We initialize `index = 0`, and once we find `nums[i] != nums[index]` (we find a new start), incrementing `index` (increase the valid array by one) and replacing new rear element of the valid array with `nums[i]`. Note that since `nums` is sorted, we are able to remove all the duplicates in one traversal.

However, the edge case should be taken care of: `nums.size() == 0`.
```C++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.size() == 0)
            return 0;
        
        int index = 0;
        for(int i = 0; i < nums.size(); i++){
            if(nums[index] != nums[i]){
                nums[++index] = nums[i];
            }
        }
        return index+1;
    }
};
```

## 3. Classic Solutions
A beautiful solution employs the idea that the number of duplicates also means the position which the next different number to replace.
```C++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int count = 0;
        for(int i = 1; i < nums.size(); i++){
            if(nums[i] == nums[i-1])
                count++;
            else
                nums[i-count] = nums[i];
        }
        return nums.size() - count;
    }
};
```