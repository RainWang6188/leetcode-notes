# 90. Subsets II

## Description

Given an integer array `nums` that may contain **duplicates**, return all possible subsets (the power set).

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example:**
```
Input: [4,4,4,1,4]
Output: [[],[1],[1,4],[1,4,4],[1,4,4,4],[1,4,4,4,4],[4],[4,4],[4,4,4],[4,4,4,4]]
```

## Classic Solution

The only difference between this question and the previous one is that we count the duplicates only once during each backtracking process.

As a result, we first sort the array. And skip furthur search if `nums[i] == nums[i-1]`.

```C++
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<int> container;
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        backtracking(nums, container, res, 0);
        return res;
    }
private:
    void backtracking(vector<int>& nums, vector<int>& container, vector<vector<int>>& res, int start) {
        res.push_back(container);
        
        for(int i = start; i < nums.size(); i++) {
            if(i > start && nums[i] == nums[i-1])
                continue;
            container.push_back(nums[i]);
            backtracking(nums, container, res, i + 1);
            container.pop_back();
        }
    }
};
```