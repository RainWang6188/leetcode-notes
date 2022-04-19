# 78. Subsets

## Description

Given an integer array `nums` of **unique** elements, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

**Example:**
```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

## Classic Solution

In backtracking, we use a variable `start` to record the starting index. And at each process, we begin to push elements starting from that index.

After pushing the current element, we will perform the backtracking for the rest of the array, so we need to upate `start + 1`.

```C++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<int> container;
        vector<vector<int>> res;
        
        backtracking(nums, container, res, 0);
        
        return res;
    }
private:
    void backtracking(vector<int>& nums, vector<int>& container, vector<vector<int>>& res, int start) {
        res.push_back(container);
        
        for(int i = start; i < nums.size(); i++) {
            container.push_back(nums[i]);
            backtracking(nums, container, res, i + 1);
            container.pop_back();
        }
    }
};
```