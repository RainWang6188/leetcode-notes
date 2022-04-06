# 923. 3Sum With Multiplicity

## Description
Given an integer array `arr`, and an integer `target`, return the number of tuples `i`, `j`, `k` such that `i < j < k` and `arr[i] + arr[j] + arr[k] == target`.

As the answer can be very large, return it modulo `1e9 + 7`.

**Example:**
```
Input: arr = [1,1,2,2,3,3,4,4,5,5], target = 8
Output: 20
Explanation: 
Enumerating by the values (arr[i], arr[j], arr[k]):
(1, 2, 5) occurs 8 times;
(1, 3, 4) occurs 8 times;
(2, 2, 4) occurs 2 times;
(2, 3, 3) occurs 2 times.
```
## Classic Solution
Count the occurrence of each number.
using hashmap or array up to you.

Loop `i` on all numbers,

loop `j` on all numbers,

check if `k = target - i - j` is valid.

Add the number of this combination to result.
3 cases covers all possible combination:

- `i == j == k`
- `i == j != k`
- `i < j && j < k`

This partition is the core of this solution.

They are basically to ensure than you won't count a triple combination `(i, j, k)` twice. All `(i, j, k)`s must be unique. 

For example, if the `target = 7`, you have three numbers: `A = [2, 4, 1]`, the answer is just `1`. And here `i < j` and `j < k` to ensure that you count `(i, j, k)` only once `(i=1, j=2, k=4)`. Similarly for `i == j != k`.

**Time Complexity:**

`3 <= A.length <= 3000`, so `N = 3000`

But `0 <= A[i] <= 100`

So this solution is` O(N + 101 * 101)`

```C++
int threeSumMulti(vector<int>& arr, int target) {
    unordered_map<int, long> dict;
    for(auto& val : arr)
        dict[val]++;
    
    long res = 0;
    for(auto& it1 : dict) {
        for(auto& it2 : dict) {
            int i = it1.first;
            int j = it2.first;
            int k = target - i - j;
            if(dict.find(k) == dict.end())
                continue;
            
            if(i == j && j == k)
                res += dict[i] * (dict[i] - 1) * (dict[i] - 2) / 6;
            else if(i == j && j != k)
                res += dict[i] * (dict[i] - 1) / 2 * dict[k];
            else if(i < j && j < k)
                res += dict[i] * dict[j] * dict[k];
        }
    }
    return res % static_cast<long>(1e9+7);
}
```