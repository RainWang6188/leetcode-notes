# 57. Insert Interval

## Description
You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [start_i, end_i]` represent the start and the end of the $i^{th}$ interval and `intervals` is sorted in **ascending** order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` after the insertion.

**Example:**
```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```
## My Solution
My solution works as follows:

1. First find the position where we can insert the `newInterval` using binary search, which searchs for the index `i` where `start_i` is the smallest one of all the start times in `intervals` that is larger than the `start` (i.e. `intervals[i][0] > newInterval[0] && intervals[i-1][0] <= newInterval[0]`)

    One thing we need to keep in mind is that edge cases where the start time of `newInterval` is larger/smaller than any of the start time in `intervals`.

2. Now we can try to insert the `newInterval`. I divide it into two general cases

- 2.1 No merging is needed when inserting `newInterval`
    
    In this case, only a simple insertion is all we need. Here are the possible conditions:

    - The original `intervals` is empty: `intervals.size() == 0`
    - When there **is** a smallest `index` where `intervals[index][0] > newInterval[0]`:
        
        - `index == 0`: the new ending time is smaller than the next starting time (i.e. `(index == 0) && (newInterval[1] < intervals[index][0])`) 
        - `index > 0`: the previous ending time is smaller than the new starting time, as well as the new ending time is smaller than the next starting time (i.e. `(index > 0) && (newInterval[0] > intervals[index - 1][1]) && (newInterval[1] < intervals[index][0])`)

    - When there **isn't** any index that satisfies `intervals[index] > newInterval[0]`: the new starting time is larger than the last index's ending time (i.e. `index == intervals.size() - 1 && newInterval[0] > intervals[index][1]`)
- 2.2 Merging is needed when inserting `newInterval`

    Here we need to merge several overlapping intervals, which means we should erase some continuous intervals from `begin_index` to `end_index` and then insert a new interval `[new_start, new_end]`. Let's find the `begin_index`, `new_start` and `end_index`, `new_end` seperately.
    
    - Find `begin_index` and `new_start`
        
        There's two conditions: start erasion from the previous index (and using the previous starting time) or start erasion from the `index` (and using the starting time of `newInterval`)

        - starting from previous index and set `new_start` as `intervals[index-1][0]`
            
            - When there **is** a smallest `index` where `intervals[index][0] > newInterval[0]`:

                - `index == 0`: No previous index, no need to consider this condition.

                - `index > 0`: Only when current starting time is smaller than the previous ending time will we start erasion from previous index (i.e. `(index > 0 && newInterval[0] <= intervals[index - 1][1])`)

            - When there **isn't** any index that satisfies `intervals[index] > newInterval[0]`: simply using the previous starting time (note that here the previous index is `index` instead of `index - 1`)

        -  starting from current index
        
            Otherwise, we will start erasion from current index and set the `new_start` as `newInterval[0]`
        
    - Find `end_index` and `new_end`

        This is much easier, we start the iterator from `intervals.begin() + begin_index` and increment it until we find `end_index` where the new ending time of `newInterval` is smaller than the `intervals[end_index][1]`

I think my code is not very consice and maybe no need to classiy so many cases...
```C++
vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
    int low = 0;
    int high = intervals.size() - 1;
    
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        if(intervals[mid][0] <= newInterval[0])
            low = mid + 1;
        else
            high = mid;
    }
    
    int index = low;
    auto it = intervals.begin();
    if((intervals.size() == 0) || 
        (((index > 0 && newInterval[0] > intervals[index - 1][1]) || (index == 0)) && newInterval[1] < intervals[index][0]) ||
        (index == intervals.size() - 1 && newInterval[0] > intervals[index][1])) {
        if(index == intervals.size() - 1 && newInterval[0] > intervals[index][1])
            index++;
        intervals.insert(it + index, newInterval);
    }
    else{
        int new_start;
        int new_end;
        int begin_index;
        int end_index;
        
        if((index == intervals.size() - 1 && newInterval[0] >= intervals[index][0]) ||
            (index != 0 && newInterval[0] <= intervals[index - 1][1])) {
            if(index == intervals.size() - 1 && newInterval[0] >= intervals[index][0])
                index++;
            
            new_start = intervals[max(index - 1, 0)][0];
            begin_index = max(index - 1, 0);
        }
        else {
            new_start = newInterval[0];
            begin_index = index;
        }
        
        for(it = intervals.begin() + begin_index, end_index = begin_index; it != intervals.end() && newInterval[1] >= (*it)[0]; it++) {
            new_end = max(newInterval[1], (*it)[1]);
            end_index++;
        }
        intervals.erase(intervals.begin() + begin_index, intervals.begin() + end_index);
        intervals.insert(intervals.begin() + begin_index, {new_start, new_end});
    }
    
    return intervals;
}
```

## Classic Solution
Actually we don't need to classify so many cases indeed, but a single one pass is enough.

```C++
vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
    int index = 0;
    auto it = intervals.begin();
    
    while(index < intervals.size() && intervals[index][1] < newInterval[0])
        index++;
    
    while(index < intervals.size() && intervals[index][0] <= newInterval[1]) {
        newInterval[0] = min(newInterval[0], intervals[index][0]);
        newInterval[1] = max(newInterval[1], intervals[index][1]);
        intervals.erase(it + index);
    }
    
    intervals.insert(it + index, newInterval);
    
    return intervals;            
}
```

In the above code, we reuse the `intervals` and use `erase()` to remove the overlapping intervals, whose time complexity is $O(N)$. As a result, the overall time complexity is $O(N^2)$, we should avoid that by allocating new vector to store the result.

```c++
vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
    vector<vector<int>> res;
    int index = 0;
    auto it = intervals.begin();
    
    while(index < intervals.size() && intervals[index][1] < newInterval[0])
        res.push_back(intervals[index++]);
        
    while(index < intervals.size() && intervals[index][0] <= newInterval[1]) {
        newInterval[0] = min(newInterval[0], intervals[index][0]);
        newInterval[1] = max(newInterval[1], intervals[index][1]);
        index++;
    }
    
    res.push_back(newInterval);
    
    while(index < intervals.size())
        res.push_back(intervals[index++]);
    
    return res;            
}
```