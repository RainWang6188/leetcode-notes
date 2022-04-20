# 216. Combination Sum III

## Description

Find all valid combinations of `k` numbers that sum up to `n` such that the following conditions are true:

- Only numbers `1` through `9` are used.
- Each number is used **at most once**.

Return a list of all possible valid combinations. The list must not contain the same combination twice, and the combinations may be returned in any order.

**Example:**
```
Input: k = 3, n = 7
Output: [[1,2,4]]
Explanation:
1 + 2 + 4 = 7
There are no other valid combinations.
```
## My Solution

Basically the algorithm is almost the same as previous combination sum questions.

```C++
class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>> res;
        if(n > 45)
            return res;
        
        vector<int> curr;
        backtracking(k, n, curr, 0, res, 1);
        return res;
    }
private:
    void backtracking(int k, int n, vector<int>& curr, int currSum, vector<vector<int>>& res, int start) {
        if(!k) {
            cout << endl;
            if(currSum == n)
                res.push_back(curr);
            return;
        }
        else if(currSum > n)
            return;

        for(int i = start; i <= 9; i++) {
            currSum += i;
            curr.push_back(i);
            backtracking(k-1, n, curr, currSum, res, i + 1);
            curr.pop_back();
            currSum -= i;
        }
    }
};
```