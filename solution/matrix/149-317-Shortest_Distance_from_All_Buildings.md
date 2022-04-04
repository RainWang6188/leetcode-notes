# 317. Shortest Distance from All Buildings

## Description
You want to build a house on an empty land which reaches all buildings in the shortest amount of distance. You can only move up, down, left and right. You are given a 2D `grid` of values `0`, `1` or `2`, where:

- Each `0` marks an empty land which you can pass by freely.
- Each `1` marks a building which you cannot pass through.
- Each `2` marks an obstacle which you cannot pass through.

## Classic Solution
A very smart solution is to start BFS from each `1` and update(add up) the distances to each `0`.

We should add the restriction as `totalVisited[i][j] == totalBuildings` to make sure the current position can be reached from all the buildings.

```C++
int shortestDistance(vector<vector<int>> &grid) {
    if(grid.empty())
        return 0;

    int minDist = INT_MAX;
    int totalBuildings = 0;
    int m = grid.size();
    int n = grid[0].size();
    vector<vector<int>> dist(m, vector<int>(n, 0));
    vector<vector<int>> totalVisited(m, vector<int>(n, 0));

    for(int i = 0; i < m; i++) {
        for(int j = 0; j < n; j++) {
            if(grid[i][j] == 1) {
                vector<vector<int>> visited(m, vector<int>(n, 0));
                vector<vector<int>> dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
                int currDist = 0;
                totalBuildings++;
                queue<pair<int, int>> q;
                for(auto& dir : dirs)
                    q.push(make_pair(i + dir[0], j + dir[1]));

                while(!q.empty()) {
                    int currSize = q.size();
                    currDist++;
                    while(currSize--) {
                        int currX = q.front().first;
                        int currY = q.front().second;
                        q.pop();

                        if(currX < 0 || currX >= m || currY < 0 || currY >= n || grid[currX][currY] != 0 || visited[currX][currY])
                            continue;
                        
                        visited[currX][currY] = 1;
                        totalVisited[currX][currY]++;
                        dist[currX][currY] += currDist;

                        for(auto& dir : dirs) 
                            q.push(make_pair(currX + dir[0], currY + dir[1]));
                    }
                }
            }
        }
    }

    for(int i = 0; i < m; i++) {
        for(int j = 0; j < n; j++) {
            if(!grid[i][j] && totalVisited[i][j] == totalBuildings)
                minDist = min(minDist, dist[i][j]);
        }
    }
    return minDist;
}
```