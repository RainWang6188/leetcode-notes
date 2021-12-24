# 287. Find the Duplicate Number
## Description
Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one repeated number** in `nums`, return this repeated number.

You must solve the problem **without** modifying the array `nums` and uses only constant extra space.

**example**
```
Input: nums = [1,3,4,2,2]
Output: 2
```

## My Solution
My solution uses $O(N)$ extra memory. I use the `unordered_set` to remove the duplicates, then we can find the duplicate number from difference between the sum of the original vector `nums` and the sum of `unordered_set`.
```C++
int findDuplicate(vector<int>& nums) {
    unordered_set<int> dict(nums.begin(), nums.end());
    long dict_sum = 0;
    for(int val : dict) 
        dict_sum += val;
    
    long nums_sum = 0;
    for(int num : nums)
        nums_sum += num;
    
    return (nums_sum - dict_sum) / (nums.size() - dict.size());
}
```


## Classic Solution
### 1. Binary Search + Pigeonhole Principle
There're `n+1` elements ranging in `[1, n]`, so there must be at least two elements which are equal. Although the value is not ordered, the indices are still ordered, so we can employ index to implement the binary search.

In each iteration, the algorithm finds the number of objects that should be filled into the first `n/2` holes.

1. If this number` k >= n/2+1`, then this is complies to pigeonhole principle again, we search this subproblem.

2. otherwise, consider the remaining `n/2` holes and remaining objects to fill. Because `k <= n/2`, the number of remaining objects is `r=(n+1)-k>=n/2+1`, that is, the remaining objects and remaining `n/2` holes complies to pigeonhole principle again, we just need to search this subproblem.

Time Complexity: $O(N\log N)$. Space Complexity: $O(1)$
```C++
int findDuplicate(vector<int>& nums) {
    int low = 0, high = nums.size() - 1;
    
    while(low < high) {
        int mid = low + (high - low) / 2;
        int sum = 0;
        for(int num : nums)
            if(num <= mid)
                sum++;
        
        if(sum <= mid)
            low = mid + 1;
        else if(sum > mid)
            high = mid;
    }
    
    return low;
}
```

### 2. Floyd's cycle-finding algorithm
