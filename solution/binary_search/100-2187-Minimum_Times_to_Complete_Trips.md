# 2187. Minimum Time to Complete Trips

## Description
You are given an array `time` where `time[i]` denotes the time taken by the $i^{th}$ bus to complete **one trip**.

Each bus can make multiple trips **successively**; that is, the next trip can start **immediately** after completing the current trip. Also, each bus operates **independently**; that is, the trips of one bus do not influence the trips of any other bus.

You are also given an integer `totalTrips`, which denotes the number of trips all buses should make **in total**. Return the **minimum time** required for all buses to complete **at least** `totalTrips` trips.

## My Solution
An naive brute-force solution, which searches for the minimum time linearly:
```C++
    long long minimumTime(vector<int>& time, int totalTrips) {
        int count = 0;
        long long curr_time = 0;
        while(count < totalTrips) {
            curr_time++;
            for(auto & t : time)
                if(curr_time % t == 0)
                    count++;
        }
        return curr_time;
    }
```

## Classic Solution
Here the best solution is to use binary search.

Note that `tripsForGivenTime()` should return a `long long` integer, so the counter should also be `long long` instead of `int` to avoid overflow in that function.

```C++
class Solution {
public:
    long long minimumTime(vector<int>& time, int totalTrips) {
        long long low = 0;
        long long high = 1e14;
        
        while(low < high) {
            long long mid = low + ((high - low) >> 1);
            
            if(tripsForGivenTime(mid, time) >= totalTrips)
                high = mid;
            else
                low = mid + 1;
        }
        
        return low;
    }

private:
    long long tripsForGivenTime(const long long currTime, vector<int>& time) {
        long long count = 0;
        
        for(auto& t : time) {
            count += (currTime / t);    
        }
        
        return count;
    }
};
```