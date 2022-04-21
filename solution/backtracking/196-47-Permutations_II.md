# 47. Permutations II

## Description

Given a collection of numbers, `nums`, that might contain duplicates, return all possible **unique** permutations in any order.

**Example:**
```
Input: nums = [1,1,2]
Output:
[[1,1,2],
 [1,2,1],
 [2,1,1]]
 ```
## Classic Solution

### 1. Solution 1

This solution is quite similar to the classic solution of the [46. Permutation](https://leetcode.com/problems/permutations/).

Here are some major different.

- we sort the `nums` so that duplicates can be adjacent, which can be easier to avoid duplicate permutation

- in `backtracking`, the `nums` array are passed by value instead of by reference. That's why we don't need to `swap` back. It helps the remaining of the array remain sorted.

```C++
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> res;
        backtracking(nums, res, 0);
        sort(res.begin(), res.end());
        return res;
    }
private:
    void backtracking(vector<int> nums, vector<vector<int>>& res, int start) {
        if(start == nums.size()) {
            res.push_back(nums);
            return;
        }
        
        for(int i = start; i < nums.size(); i++) {
            if(i > start && nums[i] == nums[start])
                continue;
            swap(nums[i], nums[start]);
            backtracking(nums, res, start + 1);

        }
    }
};
```

### 2. Solution 2

Here's a solution more conformed to ordinary backtracking template.

[Here](https://leetcode-cn.com/problems/permutations-ii/solution/hot-100-47quan-pai-lie-ii-python3-hui-su-kao-lu-zh/)'s an very detailed explanation.

```C++
class Solution {
public:
    /**
     * @param nums: A list of integers.
     * @return: A list of unique permutations.
     */
    vector<vector<int> > permuteUnique(vector<int> &nums) {
        vector<vector<int> > res;
        vector<bool> visited(nums.size(),false);
        vector<int> vtmp;
        sort(nums.begin(), nums.end());
        helper(nums, visited, vtmp, res);
        return res;
    }
    
    void helper(vector<int> &nums, vector<bool> &visited, vector<int> vtmp, 
             vector<vector<int> > &res) {
        if (vtmp.size() == nums.size()) {
            res.push_back(vtmp);
            return;
        }
        
        for (int i(0); i<nums.size(); i++) {
            if (visited[i] || (i && nums[i] == nums[i-1] && !visited[i-1]))
                continue;
            vtmp.push_back(nums[i]);
            visited[i]=true;
            helper(nums, visited, vtmp, res);
            visited[i]=false;
            vtmp.pop_back();
        }
    }
};
```