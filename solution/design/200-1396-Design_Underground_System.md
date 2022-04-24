# 1396. Design Underground System


## Description

An underground railway system is keeping track of customer travel times between different stations. They are using this data to calculate the average time it takes to travel from one station to another.

Implement the `UndergroundSystem` class:

- `void checkIn(int id, string stationName, int t)`

    - A customer with a card ID equal to `id`, checks in at the station `stationName` at time `t`.

    - A customer can only be checked into one place at a time.

- `void checkOut(int id, string stationName, int t)`

    - A customer with a card ID equal to `id`, checks out from the station `stationName` at time `t`.

- `double getAverageTime(string startStation, string endStation)`
    - Returns the average time it takes to travel from `startStation` to `endStation`.
    
    - The average time is computed from all the previous traveling times from `startStation` to `endStation` that happened **directly**, meaning a check in at startStation followed by a check out from endStation.

    - The time it takes to travel from `startStation` to `endStation` **may be different** from the time it takes to travel from `endStation` to `startStation`.

    - There will be at least one customer that has traveled from startStation to endStation before getAverageTime is called.

You may assume all calls to the `checkIn` and `checkOut` methods are consistent. If a customer checks in at time `t1` then checks out at time `t2`, then `t1 < t2`. All events happen in chronological order.

## Classic Solution

Here's a splendid solution.

- Uses two `unordered_map` to record the check in and check out information respectively.

- In the `checkInMap`, there's no need to use a container like `vector` to store many check in information of a single customer: as the description puts it, "A customer can only be checked into one place at a time". So we can directly use a `pair<string, int>` to record the start station and the start time.

- In the `checkOutMap`, the key is a **route**, which we use a delimeter to concatenate the `startStation` and the `endStation`. This is a very beautiful method to save time and space.


```C++
class UndergroundSystem {
private:
    unordered_map<int, pair<string, int>> checkInMap;  // {ID, <startStation, startTime>}
    
    unordered_map<string, pair<int, int>> checkOutMap; // {Route, <totalTime, totalCount>}
public:
    UndergroundSystem() {
        
    }
    
    void checkIn(int id, string stationName, int t) {
        checkInMap[id] = make_pair(stationName, t);
    }
    
    void checkOut(int id, string stationName, int t) {
        auto& checkInInfo = checkInMap[id];
        string route = checkInInfo.first + '-' + stationName;
        checkOutMap[route].first += t - checkInInfo.second;
        checkOutMap[route].second += 1;
    }
    
    double getAverageTime(string startStation, string endStation) {
        string route = startStation + '-' + endStation;
        return static_cast<double>(checkOutMap[route].first) / checkOutMap[route].second;
    }
};

/**
 * Your UndergroundSystem object will be instantiated and called as such:
 * UndergroundSystem* obj = new UndergroundSystem();
 * obj->checkIn(id,stationName,t);
 * obj->checkOut(id,stationName,t);
 * double param_3 = obj->getAverageTime(startStation,endStation);
 */
```