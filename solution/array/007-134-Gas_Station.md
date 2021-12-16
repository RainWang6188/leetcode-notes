# 134. Gas Station
## Description
There are n gas stations along a circular route, where the amount of gas at the ith station is `gas[i]`.

You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from the $i^{th}$ station to its next $(i + 1)^{th}$ station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays `gas` and `cost`, return *the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1*. If there exists a solution, it is **guaranteed** to be **unique**.

**Example**
```
Input: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
Output: 3
Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
```

## My Solution
A brute force solution. Start from each station and check if it can reach itself in clockwise direction.

Time Complexity: $O(N^2)$ , Space Complexity: $O(1)$.
> It will receive a [TLE](https://leetcode.com/submissions/detail/602470101/) in leetcode OJ.
```c++
int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
    int start_id = -1;
    int n = gas.size();
    
    for(int start = 0; start < n; start++) {
        int tank = gas[start];
        int cur_index = start;
        int next_index = (cur_index + 1) % n;
        
        while(tank >= cost[cur_index]) {
            tank = tank - cost[cur_index] + gas[next_index];
            cur_index = next_index;
            next_index = (cur_index + 1) % n;
            
            if(cur_index == start) {
                if(tank >= 0)
                    start_id = start;
                break;
            }
            
            if(start_id != -1)
                break;
        }
    }
    
    return start_id;
}
```

## Classic Solution
### 1. Greedy
Every time we start from `start_id`, we would go as far as we can, extending `end_id` until the remaining gas in the tank is less than 0, which means we cannot complete a circuit from `start_id`. In that situation, we would check the station before `start_id` (i.e. `start_id - 1`) to see if we can start from than position. Repeat until we have checked all the stations.

A trick is that we start from `gas.size() - 1`, and it avoids some corner cases.

```C++
int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
    int start_id = gas.size() - 1;
    int end_id = 0;
    int tank = gas[start_id] - cost[start_id];

    while(start_id > end_id) {
        if(tank >= 0) {
            tank += gas[end_id] - cost[end_id];
            end_id++;
        }
        else {
            start_id--;
            tank += gas[start_id] - cost[start_id];
        }
    }
    
    return tank >= 0 ? start_id : -1;
}
```



### 2. Two Properties
- If the car starts at $A$ and can not reach $B$. Any station between $A$ and $B$ can not reach $B$. ($B$ is the first station that $A$ can not reach.)
- If the total number of gas is bigger than the total number of cost. There must be a solution.

We can use two-pass to solve this problem.

- In the first pass, we check whether this problem has a solution (property 2)
- In the second pass, we find the start station (property 1)

```c++
int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
    int tank = 0;
    int n = gas.size();

    for(int i = 0; i < gas.size(); i++)
        tank += gas[i] - cost[i];
    
    if(tank < 0)
        return -1;
    
    int start_id = 0;
    for(int i = 0, tank = 0; i < gas.size(); i++) {
        tank += gas[i] - cost[i];
        if(tank < 0) {
            start_id = i + 1;
            tank = 0;
        }
    }

    return start_id;
}
```

This can be easily modified to one-pass as follows:
```C++
int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
    int tank = 0;
    int sum_tank = 0;
    int n = gas.size();
    
    int start_id = 0;
    for(int i = 0, tank = 0; i < gas.size(); i++) {
        sum_tank += gas[i] - cost[i];
        tank += gas[i] - cost[i];
        if(tank < 0) {
            start_id = i + 1;
            tank = 0;
        }
    }

    return sum_tank < 0 ? -1 : start_id;
}
```