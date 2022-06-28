# 630. Course Schedule III

## Description

There are `n` different online courses numbered from `1` to `n`. You are given an array courses where `courses[i] = [durationi, lastDayi]` indicate that the i`th` course should be taken continuously for `durationi` days and must be finished before or on `lastDayi`.

You will start on the `1st` day and you cannot take two or more courses simultaneously.

Return the maximum number of courses that you can take.

## Classic Solution

1. Sort courses by the end date, this way, when we're iterating through the courses, we can switch out any previous course with the current one without worrying about end date.

2. Next, we iterate through each course, if we have enough days, we'll add it to our priority queue. If we don't have enough days, then we can either

    2.1 ignore this course OR

    2.2 We can replace this course with the longest course we added earlier.

The heart of the problem is **Once you make sure a course fits in, you can remove it any time later and the other courses you have added after would still fit**. So it is always safe to remove any course in the past

```C++
int scheduleCourse(vector<vector<int>>& courses) {
    if(courses.size() <= 0) return 0;
    sort(courses.begin(), courses.end(), [](const vector<int>& a, vector<int>& b) {
        return a[1] < b[1];
    });
    priority_queue<int> q;
    int sum = 0;
    for(auto i : courses) {
        sum += i[0];
        q.push(i[0]);
        if(sum > i[1]) {
            sum -= q.top();
            q.pop();
        }
    }
    return q.size();
}
```