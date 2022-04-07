# 1011. Capacity to Ship Packages Within D Days

## Description

A conveyor belt has packages that must be shipped from one port to another within `days` days.

The `ith` package on the conveyor belt has a weight of `weights[i]`. Each day, we load the ship with packages on the conveyor belt (in the order given by `weights`). We may not load more weight than the maximum weight capacity of the ship.

Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within `days` days.

**Example:**
```
Input: weights = [1,2,3,4,5,6,7,8,9,10], days = 5
Output: 15
Explanation: A ship capacity of 15 is the minimum to ship all the packages in 5 days like this:
1st day: 1, 2, 3, 4, 5
2nd day: 6, 7
3rd day: 8
4th day: 9
5th day: 10

Note that the cargo must be shipped in the order given, so using a ship of capacity 14 and splitting the packages into parts like (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) is not allowed.
```

## My Solution

A standard binary search solution.

A small corner case is that if the current maximum weight capacity is less than current `weight[i]`, then we should directly return `false` in the check function.

```C++
class Solution {
public:
    int shipWithinDays(vector<int>& weights, int days) {
        int low = 0;
        int high = 500 * 5000;
        
        while(low < high) {
            int mid = low + ((high - low) >> 1);
            
            if(shipSuccess(weights, days, mid))
                high = mid;
            else
                low = mid + 1;
        }
        
        return high;
    }
private:
    bool shipSuccess(vector<int>& weights, int days, int mid) {
        int count = 0;
        int currentWeight = 0;
        for(auto& weight : weights) {
            if(mid < weight)
                return false;
            
            currentWeight += weight;
            if(currentWeight > mid) {
                count++;
                currentWeight = weight;
            }
        }
        count++;
        if(count > days)
            return false;
        else
            return true;
    }
};
```