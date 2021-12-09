# 27. Remove Element
## Description
Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` in-place. The relative order of the elements may be changed. If there are `k` elements after removing the duplicates, then the first `k` elements of `nums` should hold the final result. It does not matter what you leave beyond the first `k` elements.

**Examples**
```
Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
Note that the five elements can be returned in any order.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

## My Solution
Since `nums` is an integer vector, we can simply use `nums.erase()` method to eliminate elements equal to `val` in one loop. However, there're a few notes need to keep in mind
- `erase()` method will remove an element at the given position and shorten the vector by one. But the cost is **expensive**.
- The ending condition in the for loop should be `it < nums.size()`, not `it != nums.size()`, or there'll be memory leakage.
- `erase()` method doesn't modify the iterator after implementing the erase operation, so if `nums[i] == val`, the next element to be examined would be `nums[i]` after erase operation.
```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        for(vector<int>::iterator it = nums.begin(); it < nums.end(); it++){
            if(*it == val){
                nums.erase(it--);
            }
        }
        return nums.size();
    }
};
```
## Classic Solutions
Since the `erase()` method is expensive, we should not use it due to the runtime and memory usage concern. A better solution would use a pointer `index`, which always points to the next position to be replaced, and we can increment it once we encounter `nums[i] != val`.
```C++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int index = 0;
        for(int i = 0; i < nums.size(); i++){
            if(nums[i] != val)
                nums[index++] = nums[i];
        }
        return index;
    }
};
```
