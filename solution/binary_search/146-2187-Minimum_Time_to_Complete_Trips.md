# 2187. Minimum Time to Complete Trips

## Description
You are given an array `time` where `time[i]` denotes the time taken by the `ith` bus to complete **one trip**.

Each bus can make multiple trips **successively**; that is, the next trip can start immediately after completing the current trip. Also, each bus operates **independently**; that is, the trips of one bus do not influence the trips of any other bus.

You are also given an integer `totalTrips`, which denotes the number of trips all buses should make in total. 

Return the minimum time required for all buses to complete at least `totalTrips` trips.

**Example:**
```
Input: time = [1,2,3], totalTrips = 5
Output: 3
Explanation:
- At time t = 1, the number of trips completed by each bus are [1,0,0]. 
  The total number of trips completed is 1 + 0 + 0 = 1.
- At time t = 2, the number of trips completed by each bus are [2,1,0]. 
  The total number of trips completed is 2 + 1 + 0 = 3.
- At time t = 3, the number of trips completed by each bus are [3,1,1]. 
  The total number of trips completed is 3 + 1 + 1 = 5.
So the minimum time needed for all buses to complete at least 5 trips is 3.
```

## My Solution
Using binary search to find the minimum valid time needed.

```C++
long long minimumTime(vector<int>& time, int totalTrips) {
    long long low = 0;
    long long high = 1e14;
    
    while(low < high) {
        long long mid = low + ((high - low) >> 1);
        long long count = 0;
        for(auto& t : time) {
            count += mid / t;
        }
        
        if(count < totalTrips) 
            low = mid + 1;
        else
            high = mid;
    }
    return high;
}
```