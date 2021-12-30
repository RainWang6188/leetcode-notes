# 56. Merge Intervals

## Description
Given an array of intervals where `intervals[i] = [start_i, end_i]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

**Examples:**
```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```
## My Solution
A one pass solution, which is basically the same as the solution to the  [Insert Interval](https://leetcode.com/problems/insert-interval/) problem.

```C++
vector<vector<int>> merge(vector<vector<int>>& intervals) {
    vector<vector<int>> res;
    sort(intervals.begin(), intervals.end(), my_cmp);
    
    int index = 0;
    while(index < intervals.size()) {
        if(index == intervals.size() - 1 || intervals[index][1] < intervals[index + 1][0])
            res.push_back(intervals[index++]);
        else {
            int start_time = intervals[index][0];
            int end_time = intervals[index][1];
            
            while(index < intervals.size() && end_time >= intervals[index][0])
                end_time = max(end_time, intervals[index++][1]);
        
            res.push_back({start_time, end_time});
        }
    }
    return res;
}

static bool my_cmp(vector<int>& interval_1, vector<int>& interval_2) {
    return interval_1[0] < interval_2[0];
}
```