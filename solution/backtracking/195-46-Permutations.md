# 46. Permutations

## Description

Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in any order.

**Example:**
```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

## My Solution

My solution is quite straight forward. Everytime, I check if the current number has been put into the `curr`, if not, I will put it in and search forward.

Since finding if a number exist in a vector takes `O(N)`, I used an `unordered_set` to store all of the elements in the `curr`, so that this the time complexity of this check will be `O(1)` on average.

```C++
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        unordered_set<int> currSet;
        vector<int> curr;
        vector<vector<int>> res;
        backtracking(nums, curr, currSet, res);
        return res;
    }
private:
    void backtracking(vector<int>& nums, vector<int>& curr, unordered_set<int>& currSet, vector<vector<int>>& res) {
        if(curr.size() == nums.size()) {
            res.push_back(curr);
            return;
        }
        
        for(int& num : nums) {
            if(currSet.find(num) != currSet.end())
                continue;
            curr.push_back(num);
            currSet.insert(num);
            backtracking(nums, curr, currSet, res);
            currSet.erase(num);
            curr.pop_back();
        }
    }
};
```

## Classic Solution

Here's a beautiful solution.

The algorithm can be illustrated by the following picture clearly:

![demo](https://media.geeksforgeeks.org/wp-content/cdn-uploads/NewPermutation.gif)

Basically, we fix the element on each position one by one from left to right. Here the `start` parameter is used to identify the current to be fixed index.

Since elements of previous indices (i.e. `[0, start)`) have been fixed, so we will try every possible elements from behind.

Here's the code:

```C++
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> res;
        backtracking(nums, res, 0);
        return res;
    }
private:
    void backtracking(vector<int>& nums, vector<vector<int>>& res, int start) {
        if(start == nums.size()) {
            res.push_back(nums);
            return;
        }
        
        for(int i = start; i < nums.size(); i++) {
            swap(nums[i], nums[start]);
            backtracking(nums, res, start + 1);
            swap(nums[i], nums[start]);
        }
    }
};
```