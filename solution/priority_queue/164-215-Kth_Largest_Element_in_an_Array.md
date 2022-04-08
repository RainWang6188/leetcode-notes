# 215. Kth Largest Element in an Array

## Description

Given an integer array `nums` and an integer `k`, return the `kth` largest element in the array.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

**Example:**
```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

## My Solution

### 1. Sort
```C++
int findKthLargest(vector<int>& nums, int k) {
    sort(nums.begin(), nums.end(), greater<int>());
    return nums[k-1];
}
```

### 2. Priority Queue
```C++
int findKthLargest(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> pq;
    for(auto& num : nums)
        pq.push(num);
    while(pq.size() > k)
        pq.pop();
    return pq.top();
}
```


## Classic Solution

Using the parition operation to find the `kth` largest element.

This operation is also used in quickSort.

```C++
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        int left = 0;
        int right = nums.size() - 1;
        while(1) {
            int index = partition(nums, left, right);
            if(index > k - 1)
                right = index - 1;
            else if(index < k - 1)
                left = index + 1;
            else
                return nums[index];
        }
        return -1;
    }
private:
    int partition(vector<int>& nums, int left, int right) {
        if(left >= right)
            return left;
        int pivot = nums[right];
        int i = left - 1;
        for(int j = left; j < right; j++) {
            if(nums[j] >= pivot) {
                swap(nums[j], nums[++i]);
            }
        }
        swap(nums[++i], nums[right]);
        return i;
    }
};
```