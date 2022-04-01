# 329. Longest Increasing Path in a Matrix

## Description
Given an `m x n` integers `matrix`, return the length of the longest increasing path in `matrix`.

From each cell, you can either move in four directions: left, right, up, or down. You may not move diagonally or move outside the boundary (i.e., wrap-around is not allowed).

**Example:**
```
Input: matrix = [[9,9,4],[6,6,8],[2,1,1]]
Output: 4
Explanation: The longest increasing path is [1, 2, 6, 9].
```

## Classic Solution
A standard DFS with memorization.

The key observation is that the sequence is strictly increasing, so it can not have loops. So we have the following:

longest(i,j) = longest increasing path from (i,j) to (k,l) + longest(k,l)

where longest(i,j) is longest increasing path starting with (i,j).

As a result, we can store each longest(i,j) after searching, which avoids redundant searching.

```C++
class Solution {
public:
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        
        vector<vector<int>> visited(m, vector<int>(n, 0));
        
        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++) {
                maxLen = max(maxLen, dfs(matrix, i, j, visited));
            }
        
        return maxLen;
    }
private:
    int maxLen = 0;
    
    int dfs(vector<vector<int>>& matrix, int x, int y, vector<vector<int>>& visited) {
        int m = matrix.size();
        int n = matrix[0].size();
        
        if(visited[x][y])
            return visited[x][y];
        
        vector<vector<int>> dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        
        int currLen = 1;
        for(int i = 0; i < 4; i++) {
            int nextX = x + dirs[i][0];
            int nextY = y + dirs[i][1];
            
            if(nextX < 0 || nextX >= m || nextY < 0 || nextY >= n || matrix[nextX][nextY] <= matrix[x][y])
                continue;
            
            currLen = max(currLen, 1 + dfs(matrix, nextX, nextY, visited));
        }
        visited[x][y] = currLen;
        return currLen;
    }
};
```