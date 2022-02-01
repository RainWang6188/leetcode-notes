# 200. Number of Islands

## Description
Given an `m x n` 2D binary grid grid which represents a map of `'1'`s (land) and ~s (water), return the number of islands.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example:**
```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```
 
## My Solution
### 1. DFS
Replacing the searched island with `'#'` in the `grid[][]`.
```c++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        int count = 0;
        
        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++)
                if(grid[i][j] == '1') {
                    count++;
                    dfs(grid, i, j);
                }
        
        return count;
    }
    
private:
    void dfs(vector<vector<char>>& grid, int x, int y) {
        int m = grid.size();
        int n = grid[0].size();
        
        if(x < 0 || x >= m || y < 0 || y >= n || grid[x][y] != '1')
            return;
        
        grid[x][y] = '#';
        
        dfs(grid, x-1, y);
        dfs(grid, x+1, y);
        dfs(grid, x, y-1);
        dfs(grid, x, y+1);
    }
};
```

### 2. BFS

```c++
void bfs(vector<vector<char>>& grid, int x, int y) {
    int m = grid.size();
    int n = grid[0].size();
    queue<pair<int, int>> q;
    q.push(make_pair(x, y));
    
    while(!q.empty()) {
        int cur_x = q.front().first;
        int cur_y = q.front().second;
        q.pop();
        
        grid[cur_x][cur_y] = '#';
        
        if(cur_x - 1 >= 0 && grid[cur_x-1][cur_y] == '1')
            q.push(make_pair(cur_x-1, cur_y));
        if(cur_x + 1 < m && grid[cur_x+1][cur_y] == '1')
            q.push(make_pair(cur_x+1, cur_y));
        if(cur_y - 1 >= 0 && grid[cur_x][cur_y-1] == '1')
            q.push(make_pair(cur_x, cur_y-1));
        if(cur_y + 1 < n && grid[cur_x][cur_y+1] == '1')
            q.push(make_pair(cur_x, cur_y+1));
    }
```