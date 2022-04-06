# 1539. Kth Missing Positive Number


## Description
Given an array `arr` of positive integers sorted in a strictly increasing order, and an integer `k`.

Find the kth positive integer that is missing from this array.

**Example:**
```
Input: arr = [2,3,4,7,11], k = 5
Output: 9
Explanation: The missing positive integers are [1,5,6,8,9,10,12,13,...]. The 5th missing positive integer is 9.
```

## My Solution
Kind of brute force, not optimal.
```C++
int findKthPositive(vector<int>& arr, int k) {
    unordered_set<int> mySet(arr.begin(), arr.end());
    for(int count = 0, i = 1; count < k; i++) {
        if(mySet.find(i) == mySet.end())
            if(++count == k)
                return i;
    }
    return -1;
}
```

## Classic Solution

Here we have three sorted sequences:

let `n` be `len(A)`, then
**1st** sorted sequence is array values:
```
A[0], A[1], ... A[n-1]
```
**2nd** is indices:
```
0, 1, 2, ... n - 1
```
The nice part about this question: the indices can help us to get all the positive numbers in sorted order (`i + 1` if `i` denotes the index)

Hence, `A[i] - (i + 1)` will be # of missing positives at index `i`, and this will be our **3rd** sorted sequences

i.e.
```
A:            [1,3,4,6]
A[i] - (i+1): [0,1,1,2]
```
let's call array `A[i]-(i+1)` as `B`, which is `[0, 1, 1, 2]` above, then `B[i]` represents how many missing positives so far at index `i`.

So the question becomes: finding the largest index of array `B` so that `B[j]` is smaller than `K`.

It is the same as finding first/last occurrence 34. find-first-and-last-position-of-element-in-sorted-array
```
l, r = 0, len(B)
while l < r:
    m = (r + l) / 2
    if B[m] < K:
        l = m + 1
    else:
        r = m
```
After while loop is stopped, `l - 1` is our target index because, `B[l - 1]` represents how many positive is missing at index `l - 1` that is smaller than `K`, so result is A[l -1](the largest number in `A` that is less than result) + K - B[l - 1](offset, how far from result) = (A[l - 1]) + (k - (A[l - 1] - l)) = l + k

```C++
int findKthPositive(vector<int>& arr, int k) {
    int low = 0;
    int high = arr.size();
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        if(arr[mid] - mid - 1 < k)
            low = mid + 1;
        else
            high = mid;
    }
    return high + k;
}
```

`l` is the first index that gives at least `k` missing numbers. It may have more missing numbers than we need, so we are actually interested in index `l - 1`.
At index `l - 1`, we have `A[l-1] - (l-1) - 1` missing numbers
so after index `l - 1` , we need to find `k - (A[l-1] - (l-1) - 1)` missing numbers, i.e. `k - A[l-1] + l` missing numbers
At index `l - 1`, our number is `A[l-1]`. Add them up, the target number will be `A[l-1] + k - A[l-1] + l`, i.e. `k + l`;