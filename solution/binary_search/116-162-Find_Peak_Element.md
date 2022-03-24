# 162. Find Peak Element

## Description
A peak element is an element that is strictly greater than its neighbors.

Given an integer array `nums`, find a peak element, and return its index. If the array contains multiple peaks, return the index to any of the peaks.

You may imagine that `nums[-1] = nums[n] = -∞`.

You must write an algorithm that runs in `O(log n)` time.

**Example:**
```
Input: nums = [1,2,1,3,5,6,4]
Output: 5
Explanation: Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.
```

## Classic Solution
Most people have figured out the binary search solution but are not able to understand how its working. I will try to explain it simply. 

What we are essentially doing is going in the direction of the rising slope(by choosing the element which is greater than current). 

How does that guarantee the result? Think about it, there are 2 possibilities.

a) rising slope has to keep rising till end of the array

b) rising slope will encounter a lesser element and go down.

In both scenarios we will have an answer. In a) the answer is the end element because we take the boundary as -INFINITY b) the answer is the largest element before the slope falls. Hope this makes things clearer.

In all, we always step in the direction of ascent.
```C++
int findPeakElement(vector<int>& nums) {
    int low = 0;
    int high = nums.size() - 1;
    while(low < high) {
        int mid1 = low + ((high - low ) >> 1);
        int mid2 = mid1 + 1;
        
        if(nums[mid1] < nums[mid2])
            low = mid2;
        else
            high = mid1;
    }
    return low;
}
```