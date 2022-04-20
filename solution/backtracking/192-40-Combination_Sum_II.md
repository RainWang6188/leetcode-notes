# 40. Combination Sum II

## Description

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

Note: The solution set must not contain duplicate combinations.
 
**Example:**
```
Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

## My Solution

Almost the same method.

We first sort the `candidates` array and then skip the same element during backtracking. Since if we encounter an element for the first time, its further search will cover all the possibilities of any time of the occurrance of that element.

Also, we use `start` variable and update it as `start+1` to make sure each number will be used at most once in a combination.

```C++
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
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
            if(i > start && candidates[i] == candidates[i-1])
                continue;
            currSum += candidates[i];
            curr.push_back(candidates[i]);
            backtracking(candidates, target, curr, currSum, res, i + 1);
            curr.pop_back();
            currSum -= candidates[i];
        }
    }
};
```