# 1802. Maximum Value at a Given Index in a Bounded Array

## Description
You are given three positive integers: `n`, `index`, and `maxSum`. You want to construct an array `nums` (`0`-indexed) that satisfies the following conditions:

- `nums.length == n`
- `nums[i]` is a positive integer where `0 <= i < n`.
- `abs(nums[i] - nums[i+1]) <= 1` where `0 <= i < n-1`.
- The sum of all the elements of `nums` does not exceed `maxSum`.
- `nums[index]` is maximized.

Return `nums[index]` of the constructed array.

Note that `abs(x)` equals `x` if `x >= 0`, and `-x` otherwise.

**Example:**
```
Input: n = 4, index = 2,  maxSum = 6
Output: 2
Explanation: nums = [1,2,2,1] is one array that satisfies all the conditions.
There are no arrays that satisfy all the conditions and have nums[2] == 3, so 2 is the maximum nums[2].
```

## My Solution
The idea is correct, but testing if the `mid` is valid is still not efficient enough.

```C++
int maxValue(int n, int index, int maxSum) {
    int low = 0;
    int high = maxSum;
    while(low < high) {
        int mid = high - ((high - low) >> 1);
        int prev = mid;
        long sum = mid;
        for(int i = index + 1; i < n; i++) {
            sum += max(1, prev - 1);
            prev = max(1, prev - 1);
        }
            
        prev = mid;
        for(int i = index - 1; i >= 0; i--) {
            sum += max(1, prev - 1);
            prev = max(1, prev - 1);
        }
        
        if(sum > maxSum)
            high = mid - 1;
        else
            low = mid;
    }
    return high;
}
```

## Classic Solution
Here're some tricks of this elegant solution:

- `maxSum -= n`, then all elements needs only to valid `nums[i] >= 0` instead of many `1`s.
- testing the validity of `mid` in a more efficient way, where we calculate the sum using the property of arithmetic sequence

    - On the left, `A[0] = max(a - index, 0)`,
    - On the right, `A[n - 1] = max(a - ((n - 1) - index), 0)`,

    - The sum of arithmetic sequence `{b, b+1, ....a}`,
equals to `(a + b) * (a - b + 1) / 2`.

```C++
int maxValue(int n, int index, int maxSum) {
    maxSum -= n;
    int low = 0;
    int high = maxSum;
    while(low < high) {
        int mid = high - ((high - low) >> 1);
        long sum = 0;
        int leftBoundary = max(0, mid - index);
        int rightBoundary = max(0, mid - (n - 1 - index));
        sum += static_cast<long>((mid + leftBoundary)) * (mid - leftBoundary + 1) / 2;
        sum += static_cast<long>((mid + rightBoundary)) * (mid - rightBoundary + 1) / 2;
        sum -= mid;
        
        if(sum > maxSum)
            high = mid - 1;
        else
            low = mid;
    }
    return high + 1;
}
```