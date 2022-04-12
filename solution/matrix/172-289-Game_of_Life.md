# 289. Game of Life

## Description

According to Wikipedia's article: "The Game of Life, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

The board is made up of an `m x n` grid of cells, where each cell has an initial state: **live** (represented by a `1`) or **dead** (represented by a `0`). Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

- Any live cell with fewer than two live neighbors dies as if caused by under-population.
- Any live cell with two or three live neighbors lives on to the next generation.
- Any live cell with more than three live neighbors dies, as if by over-population.
- Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

The next state is created by applying the above rules **simultaneously** to every cell in the current state, where births and deaths occur simultaneously. Given the current state of the `m x n` grid board, return the next state.

**Example:**
```
Input: board = [[0,1,0],[0,0,1],[1,1,1],[0,0,0]]
Output: [[0,0,0],[1,0,1],[0,1,1],[0,1,0]]
```

## My Solution

A naive solution which uses extra memory to store the result board state.

```C++
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        int m = board.size();
        int n = board[0].size();
        
        vector<vector<int>> res(m, vector<int>(n, 0));
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                int liveNeighborCount = liveNeighbors(board, i, j);
                if((board[i][j] && liveNeighborCount >= 2 && liveNeighborCount <= 3) || (!board[i][j] && liveNeighborCount == 3))
                    res[i][j] = 1;
                else
                    res[i][j] = 0;
            }
        }
        board = res;
    }
private:
    int liveNeighbors(vector<vector<int>>& board, int x, int y) {
        int m = board.size();
        int n = board[0].size();
        if(x < 0 || x >= m || y < 0 || y >= n)
            return INT_MIN;
        
        int count = 0;
        vector<vector<int>> dirs = {{-1, 1}, {-1, 0}, {-1, -1}, {0, 1}, {0, -1}, {1, 1}, {1, 0}, {1, -1}};
        
        for(auto& dir : dirs) {
            int nextX = x + dir[0];
            int nextY = y + dir[1];
            
            if(nextX < 0 || nextX >= m || nextY < 0 || nextY >= n)
                continue;
            
            count += board[nextX][nextY];
        }
        return count;
    }
};
```

## Classic Solution

Here's an in-place solution by [yavinci](https://leetcode.com/problems/game-of-life/discuss/73223/Easiest-JAVA-solution-with-explanation).

To solve it in place, we use 2 bits to store 2 states:
```
[2nd bit, 1st bit] = [next state, current state]
```
- `00`:  dead (next) <- dead (current)
- `01`:  dead (next) <- live (current)  
- `10`:  live (next) <- dead (current)  
- `11`:  live (next) <- live (current) 

In the beginning, every cell is either `00` or `01`.

Notice that 1st state is independent of 2nd state.

Imagine all cells are instantly changing from the 1st to the 2nd state, at the same time.

Let's count # of neighbors from 1st state and set 2nd state bit.

Since every 2nd state is by default dead, no need to consider transition `01 -> 00`.

In the end, delete every cell's 1st state by doing `>> 1`.

For each cell's 1st bit, check the 8 pixels around itself, and set the cell's 2nd bit.

- Transition `01 -> 11`: when board == 1 and lives >= 2 && lives <= 3.
- Transition `00 -> 10`: when board == 0 and lives == 3.

To get the current state, simply do
`board[i][j] & 1`

To get the next state, simply do
`board[i][j] >> 1`


```C++
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        int m = board.size();
        int n = board[0].size();
        
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                int count = liveNeighbors(board, i, j);
                if(board[i][j] && count >= 2 && count <= 3)
                    board[i][j] = 0b11;
                else if(!board[i][j] && count == 3)
                    board[i][j] = 0b10;
            }
        }
        
        for(auto& row : board)
            for(auto& state : row)
                state >>= 1;
    }
private:
    int liveNeighbors(vector<vector<int>>& board, int x, int y) {
        int m = board.size();
        int n = board[0].size();
        
        int count = 0;
        for(int i = max(0, x - 1); i <= min(m - 1, x + 1); i++) {
            for(int j = max(0, y - 1); j <= min(n - 1, y + 1); j++) {
                count += board[i][j] & 1;
            }
        }
        count -= board[x][y] & 1;
        return count;
    }
};
```