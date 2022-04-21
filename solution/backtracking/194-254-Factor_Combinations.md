# 254. Factor Combinations

## Description

Numbers can be regarded as product of its factors. For example,
```
8 = 2 x 2 x 2;
  = 2 x 4.
```
Write a function that takes an integer `n` and return all possible combinations of its factors.

**Example:**
```
Input: 12
Output: 
[
  [2, 6],
  [2, 2, 3],
  [3, 4]
]
Explanation:
2*6 = 12
2*2*3 = 12
3*4 = 12
```

## My Solution

Before backtracking, I first stored all of the possible factors in a vector `factors`, so that we don't need to search for each of the integer ranging from $(1, n)$ and checking if it is a factor or not.

One thing we need to be careful is that here the type of `currProd` should be `long`, otherwise, it may cause integer overflow.

```C++
class Solution {
public:
    vector<vector<int>> getFactors(int n) {
        vector<int> factors;
        for(int i = 2; i < n; i++)
            if(n % i == 0)
                factors.push_back(i);
        vector<vector<int>> res;
        vector<int> curr;
        backtracking(n, factors, curr, 1l, res, 0);
        return res;
    }
private:
    void backtracking(int n, vector<int>& factors, vector<int>& curr, long currProd, vector<vector<int>>& res, int start) {
        if(currProd == n) {
            res.push_back(curr);
            return;
        }
        else if(currProd > n)
            return;
        
        for(int i = start; i < factors.size(); i++) {       
            currProd *= factors[i];
            curr.push_back(factors[i]);
            backtracking(n, factors, curr, currProd, res, i);
            curr.pop_back();
            currProd /= factors[i];
        }
    }
};
```