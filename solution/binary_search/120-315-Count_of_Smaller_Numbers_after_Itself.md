# 315. Count of Smaller Numbers after itself

## Description
You are given an integer array `nums` and you have to return a new counts array. The counts array has the property where `counts[i]` is the number of smaller elements to the right of `nums[i]`.

**Example:**
```
Input: nums = [5,2,6,1]
Output: [2,1,1,0]
Explanation:
To the right of 5 there are 2 smaller elements (2 and 1).
To the right of 2 there is only 1 smaller element (1).
To the right of 6 there is 1 smaller element (1).
To the right of 1 there is 0 smaller element.
```

## Classic Solution
A very detailed explanation can be viewed [here](https://leetcode.com/problems/count-of-smaller-numbers-after-self/discuss/445769/merge-sort-CLEAR-simple-EXPLANATION-with-EXAMPLES-O(n-lg-n)).
```C++
class Solution {
public:
    vector<int> countSmaller(vector<int>& nums) {
        vector<pair<int, int>> dict;
        vector<int> res(nums.size(), 0);
        for(int i = 0; i < nums.size(); i++)
            dict.push_back(make_pair(nums[i], i));

        mergeSortAndCount(0, nums.size() - 1, dict, res);
        return res;
    }
private:
    void mergeSortAndCount(int start, int end, vector<pair<int, int>>& nums, vector<int>& res) {
        if(start >= end)
            return;
        
        int mid = start + ((end - start) >> 1);
        mergeSortAndCount(start, mid, nums, res);
        mergeSortAndCount(mid + 1, end, nums, res);
        
        int left = start;
        int right = mid + 1;
        int nRightLessThanLeft = 0;
        vector<pair<int, int>> merged;
        
        while(left <= mid && right <= end) {
            if (nums[left] > nums[right]) {
                nRightLessThanLeft++;
                merged.push_back(nums[right++]);
            }
            else {
                res[nums[left].second] += nRightLessThanLeft;
                merged.push_back(nums[left++]);
            }
        }
        
        while(left <= mid) {
            res[nums[left].second] += nRightLessThanLeft;
            merged.push_back(nums[left++]);
        }
        while(right <= end) {
            merged.push_back(nums[right++]);
        }
        
        for(int i = start, j = 0; j < merged.size(); i++, j++) {
            nums[i] = merged[j];
        }
    }
};
```