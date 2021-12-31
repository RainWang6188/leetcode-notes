# 253. Meeting Rooms Ⅱ

## Description
Given an array of meeting time `intervals` consisting of start and end times `[[s1,e1],[s2,e2],...] (si < ei)`, find the minimum number of conference rooms required.

**Example 1:**
```
Input: [[0, 30],[5, 10],[15, 20]]
Output: 2
```

**Example 2:**
```
Input: [[2,7]]
Output: 1
```
## Classic Solutions
### 1. Two Pointer + Sort
The first solution is sort the start times and end times seperately, and use two pointers to traverse the arrays and maintain the `count` at the same time. The maximum `count` during the traversal is the maximum number of meeting rooms needed.
```C++
int minMeetingRooms(vector<vector<int>> &intervals) {
    vector<int> start_time;
    vector<int> end_time;

    for(int i = 0; i < intervals.size(); i++) {
        start_time.push_back(intervals[i][0]);
        end_time.push_back(intervals[i][1]);
    }

    sort(start_time.begin(), start_time.end());
    sort(end_time.begin(), end_time.end());

    int start_pointer = 0;
    int end_pointer = 0;
    int count = 0;
    int max_count = 0;

    while(start_pointer < start_time.size() && end_pointer < end_time.size()) {
        if(start_time[start_pointer] < end_time[end_pointer]){
            max_count = max(max_count, count += 1);
            start_pointer++;
        }
        else {
            count--;
            end_pointer++;
        }
    }
    return max_count;
}
```

### 2. Sweeping Line 
We sort all the starting time and ending time of the meetings into a non-descending array. Note that if two times are equal, **place the ending time prior to the starting time** (since we need to clear out the room to the next meeting so as to minimize the total rooms needed).

Then, we can traverse the array and maintain a `counter` which records the rooms needed at the same time. When we encounter a starting time, we increment the `counter`, when we encounter an ending time, we decrement the `counter`. As a result, the value of `counter` at $i^{th}$ iteration is the number of rooms needed at $i^{th}$ time. The result is the maximum `counter` during the traversal.

To differentiate the starting time from the ending time, we can use a pair structure, `1` indicates starting time and `-1` indicates ending time.

The code is as follows:

```C++
int minMeetingRooms(vector<vector<int>> &intervals) {
    vector<pair<int, int>> rooms;
    for(int i = 0; i < intervals.size(); i++) {
        rooms.push_back(make_pair(intervals[i][0], 1));
        rooms.push_back(make_pair(intervals[i][1], -1));
    }

    sort(rooms.begin(), rooms.end(), cmp);

    int count = 0;
    int max_count = -1;

    for(auto time : rooms) {
        count += time.second;
        max_count = max(max_count, count);
    }

    return max_count;
}

static bool cmp(pair<int, int> time_1, pair<int, int> time_2) {
    if(time_1.first != time_2.first)
        return time_1.first < time_2.first;
    else
        return time_1.second < time_2.second;
}

```

### 3. Priority Queue
This solution uses a priority queue to implement the traversal.

First we sort the intervals according to their starting time into non-descending order. Then, we always keep the ending time of meeting in the priority queue. Since the we will dequeue the earliest ending time  each time, it it's less than the starting time of next meeting, we don't need to use an extra room so dequeue it and push the current ending time of next meeting.

The final size of the priority queue is the maximum meeting rooms needed (because the queue will only pop the smallest one, and keep the other overlapping intervals).
> Note that a priority queue sorts in reverse to the order relation it is given (e.g. by using `std::greater<int>`, it dequeues the next smallest element)

```C++
int minMeetingRooms(vector<vector<int>>& intervals) {
    sort(intervals.begin(), intervals.end(), [](const vector<int>& a, const vector<int>& b){ return a[0] < b[0]; });
    priority_queue<int, vector<int>, greater<int>> q;
    for (auto interval : intervals) {
        if (!q.empty() && q.top() <= interval[0]) q.pop();
        q.push(interval[1]);
    }
    return q.size();
}
```
### 4. TreeMap
Another tricky implementation is to use map structure. It is quite similar to the sweeping line algorithm, but using the built-in ordering property of `map`. We traverse each intervals and increment the value of key `start_time` and decrement the value of key `end_time`. After that, we scan the element linearly in the map and record the maximum value as the maximum meeting rooms needed.

```C++
int minMeetingRooms(vector<vector<int>> &intervals) {
    map<int, int> rooms;
    for(int i = 0; i < intervals.size(); i++) {
        rooms[intervals[i][0]]++;
        rooms[intervals[i][1]]--;
    }

    int count = 0;
    int max_count = -1;

    for(auto time : rooms) {
        max_count = max(max_count, count += time.second);
    }

    return max_count;
}
```