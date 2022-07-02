# 871. Minimum Number of Refueling Stops

## Description

A car travels from a starting position to a destination which is `target` miles east of the starting position.

There are gas stations along the way. The gas stations are represented as an array `stations` where `stations[i] = [positioni, fueli]` indicates that the `ith` gas station is `positioni` miles east of the starting position and has `fueli` liters of gas.

The car starts with an infinite tank of gas, which initially has `startFuel` liters of fuel in it. It uses one liter of gas per one mile that it drives. When the car reaches a gas station, it may stop and refuel, transferring all the gas from the station into the car.

Return the minimum number of refueling stops the car must make in order to reach its destination. If it cannot reach the destination, return `-1`.

Note that if the car reaches a gas station with `0` fuel left, the car can still refuel there. If the car reaches the destination with `0` fuel left, it is still considered to have arrived.


## Classic Solution

We always use up all the remaining fuel each time, traveling as far as we can. At the same time, we save the liters of gases of each station passing by along the way to a max-heap.

Once the fuel is used up, we pop the largest amount of gas from the heap and increment the counter. Since no matter what amount of gas you refuel, it will always add up one to the counter. As a result, refueling the largest amount of gas is always optimal.

```C++
int minRefuelStops(int target, int startFuel, vector<vector<int>>& stations) {
    int currFuel = startFuel;
    int currDist = 0;
    int count = 0;
    int n = stations.size();
    int index = 0;

    priority_queue<int> q;
    while(currDist < target) {
        if(!currFuel) {
            if(!q.empty()) {
                currFuel += q.top();
                q.pop();
                count++;
            }
            else
                return -1;
        }
        
        currDist += currFuel;
        currFuel = 0;
        while(index < n && stations[index][0] <= currDist)
            q.push(stations[index++][1]);
    }
    return count;
}
```