# 238. Product of Array Except Self

## Description
Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of nums *except* `nums[i]`.

The product of any prefix or suffix of nums is **guarantee**d to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

**example**
```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

## My Solution
I didn't figure out the $O(N)$-time solution. My original naive brute force solution will receive TLE.


## Classic Solution
This solution uses two passes to compute the product of all the `left` and the `right` of a specific element. Here's an example.

Given numbers `[2, 3, 4, 5]`, regarding the third number `4`, the product of array except `4` is `2*3*5` which consists of two parts: left `2*3` and right `5`. The product is `left*right`. We can get lefts and rights:
```
Numbers:     2    3    4     5
Lefts:            2  2*3 2*3*4
Rights:  3*4*5  4*5    5      
```

Let’s fill the empty with 1:
```
Numbers:     2    3    4     5
Lefts:       1    2  2*3 2*3*4
Rights:  3*4*5  4*5    5     1
```
Below is the code, its time complexity is $O(N)$ and space complexity is $O(N)$.
```c++
vector<int> productExceptSelf(vector<int>& nums) {
    vector<int> left(nums.size(), 1);
    vector<int> right(nums.size(), 1);
    
    for(int i = 1; i < nums.size(); i++) 
        left[i] = nums[i - 1] * left[i - 1];
    
    for(int i = nums.size() - 2; i >= 0; i--) 
        right[i] = nums[i + 1] * right[i + 1];
    
    for(int i = 0; i < nums.size(); i++)
        left[i] *= right[i];
    
    return left;
}
```

As you can see, we use two vectors `left` and `right` to store the product of lefts and rights. And compute the result product in `left` and return it. 

In order to reduce its space complexity to $O(1)$, we can use a variable to replace the `right` vector.
```C++
vector<int> productExceptSelf(vector<int>& nums) {
    vector<int> left(nums.size(), 1);
    
    for(int i = 1; i < nums.size(); i++) 
        left[i] = nums[i - 1] * left[i - 1];
    
    for(int i = nums.size() - 2, right = 1; i >= 0; i--) {
        right *= nums[i + 1];
        left[i] *= right;
        } 
    
    return left;
}
```