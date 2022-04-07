# 848. Minimize Max Distance to Gas Station

## Description

On a horizontal number line, we have gas stations at positions `stations[0]`, `stations[1]`, ..., `stations[N-1]`, where `N = stations.length`.

Now, we add `K` more gas stations so that `D`, the maximum distance between adjacent gas stations, is minimized.

Return the smallest possible value of `D`.

**Example:**
```
Input：stations = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]，K = 9
Output：0.50
Explanation：The distance between adjacent gas stations is 0.50
```

## Classic Solution

Perform binary search on the width within the boundaries, we will narrow down our search to find the minimum gap distance between stations until these boundaries differ less than our desired delta.

This is the standard way to perform binary search with double type.

```C++
class Solution {
public:
    double minmaxGasDist(vector<int> &stations, int k) {
        double delta = 1e-6;
        double low = 0;
        double high = stations.back() - stations.front();

        while(high - low > delta) {
            double mid = (low + high) / 2;
            if(checkSuccess(stations, k, mid))
                high = mid;
            else
                low = mid;
        }
        return high;
    }
private:
    bool checkSuccess(vector<int>& stations, int k, double dist) {
        int count = 0;
        for(int i = 0; i < stations.size() - 1; i++)
            count += ceil((stations[i+1] - stations[i]) / dist) - 1;
        return count <= k;
    }
};
```