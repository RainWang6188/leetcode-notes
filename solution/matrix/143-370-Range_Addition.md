# 370. Range Addition

## Description
Assume you have an array of length `n` initialized with all `0`’s and are given `k` update operations.

Each operation is represented as a triplet: `[startIndex, endIndex, inc]` which increments each element of subarray A`[startIndex … endIndex]` (`startIndex` and `endIndex` inclusive) with `inc`.

Return the modified array after all `k` operations were executed.

**Example:**
```
Given:
    length = 5,
    updates = [
        [1,  3,  2],
        [2,  4,  3],
        [0,  2, -2]
    ]

Output:

    [-2, 0, 3, 5, 3]
```

## Classic Solution
The brute force will cause TLE.

A very elegant solution is as follows:

- We initialize an array `res` with length `length + 1`.
- For each operation `[startIndex, endIndex, inc]` in `operations`, we do the following operation
    - `res[startIndex] += inc`
    - `res[endIndex + 1] -= inc`
    
    It means all the elements start from `startIndex` will add `inc` and all the elements start from `endIndex` will minus `inc`.
- As a result, the result value in `res` can be computed by: `res[i] = res[i] + res[i-1]`.

```C++
vector<int> getModifiedArray(int length, vector<vector<int>> &updates) {
    vector<int> res(length + 1, 0);
    for(auto& update : updates) {
        res[update[0]] += update[2];
        res[update[1]+1] += -1 * update[2];
    }

    res.pop_back();
    for(int i = 1; i < length; i++) {
        res[i] += res[i-1];
    }
    return res;
}
```