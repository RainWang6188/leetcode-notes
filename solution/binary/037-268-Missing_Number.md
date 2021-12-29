# 268. Missing Number

## Description

## My Solution
### 1. XOR
We know that any number XOR itself will be zero, so we can use this property to calculate the missing number.

```C++
int missingNumber(vector<int>& nums) {
    int n = nums.size();
    int res = n;
    for(int i = 0; i < n; i++) 
        res ^= (i ^ nums[i]);
    
    return res;
    }
```
### 2. Sum
Another easy way to find the missing number is subtract the sum of array `nums` from $\sum_{i=0}^n i$.
```C++
int missingNumber(vector<int>& nums) {
    int n = nums.size();
    int res = n;
    
    for(int i = 0; i < n; i++) {
        res += i;
        res -= nums[i];
    }
    return res;
}
```

## Classic Solution
One more solution is to sort the array first, and find the first element `nums[i]` that satisfies `nums[i] < i` (i.e. the missing element) using binary search.
```C++
int missingNumber(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    
    int low = 0;
    int high = nums.size() - 1;
    
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        
        if(nums[mid] <= mid)
            low = mid + 1;
        else
            high = mid;
    } 
    return nums[low] == low ? nums.size() : low;
}
```
Note that when the while-loop terminates, we need to check if `nums[low] == low`. If it is the case, then the missing element is the `n`.