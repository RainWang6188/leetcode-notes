# 435. Non-overlapping Intervals
## Description
Given an array of intervals `intervals` where `intervals[i] = [starti, endi]`, return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

**Example:**
```
Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
Output: 1
Explanation: [1,3] can be removed and the rest of the intervals are non-overlapping.
```
## My Solution
This is a classic algorithm known as [Interval Scheduling](https://en.wikipedia.org/wiki/Interval_scheduling#), which can be used in finding the maximum number of non-overlapping intervals.

The following greedy algorithm, called *Earliest deadline first scheduling*, does find the optimal solution for **unweighted** single-interval scheduling:

1. Select the interval, `x`, with the **earliest finishing time**.
2. Remove `x`, and all intervals intersecting `x`, from the set of candidate intervals.
3. Repeat until the set of candidate intervals is empty.

Why we need to sort the interval according to their ending time? That is owing to the fact that when two intervals `X` and `Y` is overlapping, keeping the one with earlier ending time results in higher possibility of putting more other intervals behind it.

```C++
int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    sort(intervals.begin(), intervals.end(), cmp);
    int index = 0;        
    int count = 0;
    int end_time = 0;
    
    while(index < intervals.size()) {
        end_time = intervals[index][1];
        count++;
        
        while(index < intervals.size() && intervals[index][0] < end_time)
            index++;
    }
    return intervals.size() - count;
}

static bool cmp(vector<int>& interval_1, vector<int>& interval_2) {
    return interval_1[1] < interval_2[1];
}
```