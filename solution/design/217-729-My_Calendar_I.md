# 729. My Calendar I

## Description

You are implementing a program to use as your calendar. We can add a new event if adding the event will not cause a double booking.

A **double** booking happens when two events have some non-empty intersection (i.e., some moment is common to both events.).

The event can be represented as a pair of integers `start` and `end` that represents a booking on the half-open interval `[start, end)`, the range of real numbers `x` such that `start <= x < end`.

Implement the MyCalendar class:

- `MyCalendar()` Initializes the calendar object.
- `boolean book(int start, int end)` Returns `true` if the event can be added to the calendar successfully without causing a double booking. Otherwise, return `false` and do not add the event to the calendar.


## My Solution

I used a `std::list` to store all the events according to the non-descreasing starting time.

Then we can try to insret the new event interval by traversing the list. If a conflict occurs, return `false`, otherwise we can insert the interval successfully.

```C++
class MyCalendar {
private:
    list<pair<int, int>> calendar;
public:
    MyCalendar() {}
    
    bool book(int start, int end) {
        auto it = calendar.begin();
        while(it != calendar.end() && it->second <= start)
            it++;
        
        if(it == calendar.end() || it->first >= end) {
            calendar.insert(it, make_pair(start, end));
            return true;
        }
        else
            return false;
    }
};
```

## Classic Solution

### 1. Flag Insertion

This method is the same as which used in [Meeting Room II](https://leetcode.cn/problems/meeting-rooms-ii/).

We use a `std::map<int, int>` to store the `<time, flag>` pairs.

- For each start time, we increment `map[start]`

- For each end time, we decrement `map[end]`

Then we traverse the whole map and find the maximum count of flags by tracking its variant.

This methods is not very efficient, but it's easy to implement and fits the variants of this question.

```C++
class MyCalendar {
private:
    map<int, int> record;   // <time, flag>
public:
    MyCalendar() {}
    
    bool book(int start, int end) {
        record[start]++;
        record[end]--;

        int maxCount = 0;
        int currCount = 0;
        for(const auto& p : record) {
            currCount += p.second;
            maxCount = max(maxCount, currCount);
        }

        if(maxCount > 1) {
            record[start]--;
            record[end]++;
            return false;
        }
        return true;
    }
};
```

### 2. Segment Tree

I'm not familiar with segment tree, but I will learn that afterwards, [here](https://leetcode.cn/problems/my-calendar-i/solution/by-ac_oier-hnjl/)'s an solution example.