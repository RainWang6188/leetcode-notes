# 410. Split Array Largest Sum

## Description
Given an array `nums` which consists of non-negative integers and an integer `m`, you can split the array into `m` non-empty continuous subarrays.

Write an algorithm to minimize the largest sum among these `m` subarrays.

**Example:**
```
Input: nums = [7,2,5,10,8], m = 2
Output: 18
Explanation:
There are four ways to split nums into two subarrays.
The best way is to split it into [7,2,5] and [10,8],
where the largest sum among the two subarrays is only 18.
```

## My Solution
A standard binary search solution.

We narrow down the possible largest sum of subarrays and compare the count of the subarrays with `m`.
- `count > m`: current largest sum cannot hold all the elements, we need to increase it.
- `count <= m`: current largest sum is valid, we try to narrow it down.

```C++
class Solution {
public:
    int splitArray(vector<int>& nums, int m) {
        int low = 0;
        int high = 1e9;
        while(low < high) {
            int mid = low + ((high - low) >> 1);
            if(checkSuccess(nums, m, mid))
                high = mid;
            else
                low = mid + 1;
        }
        return high;
    }
private:
    bool checkSuccess(vector<int>& nums, int m, int largestSum) {
        int currSum = 0;
        int count = 0;
        for(auto& num : nums) {
            if(largestSum < num)
                return false;
            
            currSum += num;
            if(currSum > largestSum) {
                currSum = num;
                count++;
            }
        }
        count++;
        return count <= m;
    }
};
```

## Classic Solution
