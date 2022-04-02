# 296. Best Meeting Point

## Description
A group of two or more people wants to meet and minimize the total travel distance. You are given a 2D `grid` of values `0` or `1`, where each `1` marks the home of someone in the group. The distance is calculated using **Manhattan Distance**, where `distance(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|`.

**Example:**
```
given three people living at (0,0), (0,4), and (2,2):

1 - 0 - 0 - 0 - 1
|   |   |   |   |
0 - 0 - 0 - 0 - 0
|   |   |   |   |
0 - 0 - 1 - 0 - 0

The point (0,2) is an ideal meeting point, as the total travel distance of 2+2+2=6 is minimal. So return 6.
```
## Classic Solution
Since we use Manhattan Distance, we can search for the minmum row index and column index seperately.

We first traverse the `grid`, for each `grid[i][j] == 1`, we push the row index and column index to `rows` and `cols` vectors respectively.

Then the minimum index is just the median, we can calculate that median and compute the sum of distance. Or we can skip finding the median, directly computing the distance.

```C++
class Solution {
public:
    /**
     * @param grid: a 2D grid
     * @return: the minimize travel distance
     */
    int minTotalDistance(vector<vector<int>> &grid) {
        int m = grid.size();
        int n = grid[0].size();

        vector<int> rows;
        vector<int> cols;

        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(grid[i][j]) {
                rows.push_back(i);
                cols.push_back(j);
                }
            }
        }

        return getDist(rows) + getDist(cols);
    }

private:
    int getDist(vector<int>& arr) {
        sort(arr.begin(), arr.end());
        int low = 0;
        int high = arr.size() - 1;
        int res = 0;
        while(low < high) {
            res += arr[high] - arr[low];
            low++;
            high--;
        }
        return res;
    }
};
```