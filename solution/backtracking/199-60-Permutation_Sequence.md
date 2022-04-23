# 60. Permutation Sequence

## Description

The set `[1, 2, 3, ..., n]` contains a total of `n!` unique permutations.

By listing and labeling all of the permutations in order, we get the following sequence for `n = 3`:
```
"123"
"132"
"213"
"231"
"312"
"321"
```
Given `n` and `k`, return the kth permutation sequence.

 

**Example:**
```
Input: n = 3, k = 3
Output: "213"
```

## My Solution

This solution employs the previously implemented solution of [31. Next Permutation](https://leetcode.com/problems/next-permutation/).

```C++
class Solution {
private:
    void nextPermutation(vector<int>& perm) {
        int index = perm.size() - 1;
        while(index > 0 && perm[index] <= perm[index-1])
            index--;
        
        if(!index) {
            reverse(perm.begin(), perm.end());
            return;
        }
        
        int nextIndex = perm.size() - 1;
        while(nextIndex >= index && perm[nextIndex] <= perm[index-1])
            nextIndex--;
        swap(perm[index-1], perm[nextIndex]);
        reverse(perm.begin() + index, perm.end());
    }
    
public:
    string getPermutation(int n, int k) {
        vector<int> perm;
        for(int i = 0; i < n; i++)
            perm.push_back(i+1);
        while(--k)
            nextPermutation(perm);
        
        string res = "";
        for(auto& val : perm)
            res += to_string(val);
        return res;
    }
};
```

## Classic Solution

We can find the digit one by one from left to right.

You can find a great explanation [here](https://leetcode-cn.com/problems/permutation-sequence/solution/hui-su-jian-zhi-python-dai-ma-java-dai-ma-by-liwei/).

Here're some key notes:

- We compute the factorial ahead to increase efficiency

- When we move iterator of STL, we should use `next(it, step)` or `prev(it, step)`, instead of directly adding them up.

- Since we compute the index using `floor` (directly truncate to integer), we should `k--` ahead.

```C++
string getPermutation(int n, int k) {
    vector<int> factorial;
    factorial.push_back(1);
    for(int i = 1; i < n; i++)
        factorial.push_back(factorial.back() * i);
    
    list<char> nums;
    for(int i = 1; i <= n; i++)
        nums.push_back(i + '0');
    
    k--;
    string res = "";
    for(int i = n - 1; i >= 0; i--) {
        int index = k / factorial[i];
        auto it = next(nums.begin(), index);
        res += *it;
        nums.erase(it);
        k -= factorial[i] * index;
    }
    return res;
}
```