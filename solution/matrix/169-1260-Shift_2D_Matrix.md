# 1260. Shift 2D Matrix

## Description
Given a 2D `grid` of size `m x n` and an integer `k`. You need to shift the `grid` `k` times.

In one shift operation:

- Element at `grid[i][j]` moves to `grid[i][j + 1]`.
- Element at `grid[i][n - 1]` moves to `grid[i + 1][0]`.
- Element at `grid[m - 1][n - 1]` moves to `grid[0][0]`.

Return the 2D grid after applying shift operation `k` times.

**Example:**
```
Input: grid = [[1,2,3],[4,5,6],[7,8,9]], k = 1
Output: [[9,1,2],[3,4,5],[6,7,8]]
```

## My Solution
Not very efficient.

```C++
vector<vector<int>> shiftGrid(vector<vector<int>>& grid, int k) {
    int m = grid.size();
    int n = grid[0].size();
    
    k = k % (m * n);
    
    while(k >= n) {
        k -= n;
        vector<int> temp = grid.back();
        grid.pop_back();
        grid.insert(grid.begin(), temp);
    }
    if(!k)
        return grid;
    
    vector<int> arr;
    for(auto& row : grid)
        for(auto& val : row)
            arr.push_back(val);
    
    while(k--) {
        int val = arr.back();
        arr.pop_back();
        arr.insert(arr.begin(), val);
    }
    
    for(int i = 0; i < arr.size(); i++) {
        grid[i/m][i%m] = arr[i];
    }
    return grid;
}
```

## Classic Solution

### 1. Resolve Index
Start traversal from the $(m * n - k)^{th}$ element (suppose `k < m * n`). Then we can insert following elements into the result matrix.
```C++
vector<vector<int>> shiftGrid(vector<vector<int>>& grid, int k) {
    int m = grid.size();
    int n = grid[0].size();
    
    vector<vector<int>> res;
    int start = m * n - k % (m * n);
    for(int i = start; i < start + m * n; i++) {
        int index = i % (m * n);
        int x = index / n;
        int y = index % n;
        if((index - start) % n == 0)
            res.push_back({});
        res.back().push_back(grid[x][y]);
    }
    return res;
}
```

### 2. Reverse
- Imagine the `grid` as a 1-D array with length `m * n`.
- Reverse the whole 1-D array
- Reverse the first `k` elements
- Reverse the remaining `m * n - k` elements


```C++
class Solution {
public:
    vector<vector<int>> shiftGrid(vector<vector<int>>& grid, int k) {
        int m = grid.size();
        int n = grid[0].size();
        
        k %= m * n;
        reverse(grid, 0, m * n - 1);
        reverse(grid, 0, k - 1);
        reverse(grid, k, m * n - 1);
        
        return grid;
    }
private:
    void reverse(vector<vector<int>>& grid, int low, int high) {
        int n = grid[0].size();
        
        while(low < high) {
            int xLow = low / n;
            int yLow = low % n;
            int xHigh = high / n;
            int yHigh = high % n;
            int temp = grid[xLow][yLow];
            grid[xLow][yLow] = grid[xHigh][yHigh];
            grid[xHigh][yHigh] = temp;
            
            low++;
            high--;
        }
    }
};
```