# 1283. Find the Smallest Divisor Given a Threshold


## Description

Given an array of integers `nums` and an integer `threshold`, we will choose a positive integer `divisor`, divide all the array by it, and sum the division's result. Find the **smallest** `divisor` such that the result mentioned above is less than or equal to `threshold`.

Each result of the division is rounded to the nearest integer greater than or equal to that element. (For example: `7/3 = 3` and `10/2 = 5`).

The test cases are generated so that there will be an answer.

**Example 1:**
```
Input: nums = [1,2,5,9], threshold = 6
Output: 5
Explanation: We can get a sum to 17 (1+2+5+9) if the divisor is 1. 
If the divisor is 4 we can get a sum of 7 (1+1+2+3) and if the divisor is 5 the sum will be 5 (1+1+1+2). 
```

## My Solution
A standard binary search solution.

- If the `sum > threshold`, the divisor is too small.
- If the `sum <= threshold`, the divisor is big enough.

```C++
int smallestDivisor(vector<int>& nums, int threshold) {
    long thresh = static_cast<long>(threshold);
    int low = 1;
    int high = 1e6;
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        long sum = 0;
        for(auto& num : nums) {
            if(num % mid) {
                sum += num / mid + 1;
            }
            else
                sum += num / mid;
        }
        
        if(sum > thresh)
            low = mid + 1;
        else
            high = mid;
    }
    return high;
}
```

## Classic Solution

There's a more elegant way to compute the sum of the division's result. Since `ceil(x) = int(x + 0.9999)`, we can add a big float `(m-1)/m` to current `num` to get the result.

```C++
int smallestDivisor(vector<int>& nums, int threshold) {
    int low = 1;
    int high = 1e6;
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        int sum = 0;
        for(auto& num : nums) { 
            sum += (num + mid - 1) / mid;
        }
        if(sum > threshold)
            low = mid + 1;
        else
            high = mid;
    }
    return high;
}
```