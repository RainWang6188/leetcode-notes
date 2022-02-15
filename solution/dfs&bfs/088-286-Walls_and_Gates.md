# 286. Walls and Gates
## Description
You are given a `m x n` 2D grid initialized with these three possible values.

- `-1` - A wall or an obstacle. 
- `0` - A gate. 
- `INF` - Infinity means an empty room. We use the value $2^{31} - 1 = 2147483647$ to represent `INF` as you may assume that the distance to a gate is less than `2147483647`. Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with `INF`.

**Example:**
```
input: 
    INF  -1  0   INF
    INF INF INF  -1
    INF  -1 INF  -1
    0    -1 INF  INF

output:
    3  -1   0   1
    2   2   1  -1
    1  -1   2  -1
    0  -1   3   4
```
## My Solution
### 1. DFS
The trick is start searching from the position `(i, j)` where `rooms[i][j] == 0`, and update `rooms[x][y]` along the way.
```c++
class Solution {
public:
    /**
     * @param rooms: m x n 2D grid
     * @return: nothing
     */
    void wallsAndGates(vector<vector<int>> &rooms) {
        int m = rooms.size();
        int n = rooms[0].size();

        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++) {
                if(rooms[i][j] == 0) {
                    dfs(rooms, i, j, 0);
                }
            }
    }

private:
    void dfs(vector<vector<int>> &rooms, int x, int y, int dist) {
        int m = rooms.size();
        int n = rooms[0].size();

        if(x < 0 || x >= m || y < 0 || y >= n || (dist != 0 && dist >= rooms[x][y]))
            return;
        
        rooms[x][y] = dist;

        dfs(rooms, x-1, y, dist+1);
        dfs(rooms, x+1, y, dist+1);
        dfs(rooms, x, y-1, dist+1);
        dfs(rooms, x, y+1, dist+1);
    }
};
```
### 2. BFS
The BFS version of the above method.
```C++
class Solution {
public:
    /**
     * @param rooms: m x n 2D grid
     * @return: nothing
     */
    void wallsAndGates(vector<vector<int>> &rooms) {
        int m = rooms.size();
        int n = rooms[0].size();

        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++) {
                if(rooms[i][j] == 0) {
                    bfs(rooms, i, j);
                }
            }
    }

private:
    void bfs(vector<vector<int>> &rooms, int x, int y) {
        int m = rooms.size();
        int n = rooms[0].size();

        queue<tuple<int, int, int>> q;
        q.push(make_tuple(x, y, 0));

        while(!q.empty()) {
            int curr_x = get<0>(q.front());
            int curr_y = get<1>(q.front());
            int curr_dist = get<2>(q.front());
            q.pop();

            if(curr_x < 0 || curr_x >= m || curr_y < 0 || curr_y >= n || (curr_dist != 0 && curr_dist >= rooms[curr_x][curr_y]))
                continue;
            
            rooms[curr_x][curr_y] = curr_dist;

            q.push(make_tuple(curr_x-1, curr_y, curr_dist+1));
            q.push(make_tuple(curr_x+1, curr_y, curr_dist+1));
            q.push(make_tuple(curr_x, curr_y-1, curr_dist+1));
            q.push(make_tuple(curr_x, curr_y+1, curr_dist+1));
        }
    }
}
```