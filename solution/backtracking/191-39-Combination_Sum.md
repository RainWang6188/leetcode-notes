# 39. Combination Sum

## Description

Given an array of **distinct** integers `candidates` and a target integer `target`, return a list of all unique combinations of candidates where the chosen numbers sum to `target`. You may return the combinations in **any order**.

The same number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

It is **guaranteed** that the number of **unique** combinations that sum up to `target` is less than 150 combinations for the given input.

**Example:**
```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

## Classic Solution

An ordinary backtracking.

A trick here to avoid not unique combinations is using the parameter `start`, which force the loop to start searching in the next subarray instead of all of the `candidates`.

```C++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<int> curr;
        vector<vector<int>> res;
        backtracking(candidates, target, curr, 0, res, 0);
        
        return res;
    }
private:
    void backtracking(vector<int>& candidates, int target, vector<int>& curr, int currSum, vector<vector<int>>& res, int start) {
        if(currSum == target) {
            res.push_back(curr);
            return;
        }
        else if(currSum > target)
            return;
        
        for(int i = start; i < candidates.size(); i++) {
            currSum += candidates[i];
            curr.push_back(candidates[i]);
            backtracking(candidates, target, curr, currSum, res, i);
            curr.pop_back();
            currSum -= candidates[i];
        }
    }
};
```