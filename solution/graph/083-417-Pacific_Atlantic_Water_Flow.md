# 417. Pacific Atlantic Water Flow

## Description
There is an `m x n` rectangular island that borders both the **Pacific Ocean** and **Atlantic Ocean**. The **Pacific Ocean** touches the island's left and top edges, and the **Atlantic Ocean** touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an `m x n` integer matrix heights where `heights[r][c]` represents the height above sea level of the cell at coordinate `(r, c)`.

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is **less than or equal** to the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return a 2D list of grid coordinates result where `result[i] = [ri, ci]` denotes that rain water can flow from cell `(ri, ci)` to **both** the Pacific and Atlantic oceans.

**Example:**
```
Input: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
Output: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
```
## Classic Solution
> The following solution takes credit to [star1993](https://leetcode.com/problems/pacific-atlantic-water-flow/discuss/90733/Java-BFS-and-DFS-from-Ocean).

A wiser solution is start searching from the boarder rather than internal lands. So it is rather **flood from ocean** rather than **flow to the ocean**.

### 1. DFS
```C++
class Solution {
public:
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        int m = heights.size();
        int n = heights[0].size();
        
        vector<vector<bool>> pacific(m, vector<bool>(n, false));
        vector<vector<bool>> atlantic(m, vector<bool>(n, false));
        
        for(int j = 0; j < n; j++) {
            dfs(heights, pacific, INT_MIN, 0, j);
            dfs(heights, atlantic, INT_MIN, m-1, j);
        }
        
        for(int i = 0; i < m; i++) {
            dfs(heights, pacific, INT_MIN, i , 0);
            dfs(heights, atlantic, INT_MIN, i, n-1);
        }
        
        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++)
                if(pacific[i][j] && atlantic[i][j])
                    res.push_back({i, j});
        
        return res;
    }
    
private:
    vector<vector<int>> res;
    
    void dfs(vector<vector<int>>& heights, vector<vector<bool>>& visited, int prev_height, int x, int y) {
        int m = heights.size();
        int n = heights[0].size();
        
        if(x < 0 || x >= m || y < 0 || y >= n || heights[x][y] < prev_height || visited[x][y])
            return;
        
        visited[x][y] = true;
        
        if(x - 1 >= 0 && heights[x-1][y] >= heights[x][y])
            dfs(heights, visited, heights[x][y], x-1, y);
        if(x + 1 < m && heights[x+1][y] >= heights[x][y])
            dfs(heights, visited, heights[x][y], x+1, y);
        if(y - 1 >= 0 && heights[x][y-1] >= heights[x][y])
            dfs(heights, visited, heights[x][y], x, y-1);
        if(y + 1 < n && heights[x][y+1] >= heights[x][y])
            dfs(heights, visited, heights[x][y], x, y+1);
    }
};
```
### 2. BFS
```C++
class Solution {
public:
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        int m = heights.size();
        int n = heights[0].size();
        
        vector<vector<bool>> pacific(m, vector<bool>(n, false));
        vector<vector<bool>> atlantic(m, vector<bool>(n, false));
        
        bfs(heights, pacific, true);
        bfs(heights, atlantic, false);
        
        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++)
                if(pacific[i][j] && atlantic[i][j])
                    res.push_back({i, j});
        
        return res;
    }
    
private:
    vector<vector<int>> res;

    void bfs(vector<vector<int>>& heights, vector<vector<bool>>& visited, bool is_pacific) {
        int m = heights.size();
        int n = heights[0].size();
        
        queue<pair<int, int>> q;
        if(is_pacific) {
            for(int i = 0; i < m; i++)
                q.push(make_pair(i, 0));
            for(int j = 0; j < n; j++)
                q.push(make_pair(0, j));
        }
        else {
            for(int i = 0; i < m; i++)
                q.push(make_pair(i, n-1));
            for(int j = 0; j < n; j++)
                q.push(make_pair(m-1, j));
        }
        
        while(!q.empty()) {
            int x = q.front().first;
            int y = q.front().second;
            q.pop();
            
            if(x < 0 || x >= m || y < 0 || y >= n || visited[x][y])
                continue;
            
            visited[x][y] = true;
            
            if(x - 1 >= 0 && heights[x-1][y] >= heights[x][y])
                q.push(make_pair(x-1, y));
            if(x + 1 < m && heights[x+1][y] >= heights[x][y])
                q.push(make_pair(x+1, y));
            if(y - 1 >= 0 && heights[x][y-1] >= heights[x][y])
                q.push(make_pair(x, y-1));
            if(y + 1 < n && heights[x][y+1] >= heights[x][y])
                q.push(make_pair(x, y+1));
        }
    }
};
```