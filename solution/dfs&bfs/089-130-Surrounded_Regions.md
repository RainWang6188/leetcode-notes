# 130. Surrounded Regions
## Description
Given an `m x n` matrix board containing `'X'` and `'O'`, *capture* all regions that are 4-directionally surrounded by `'X'`.

A region is **captured** by flipping all `'O'`s into `'X'`s in that surrounded region.

**Example:**
```
Input: board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]
Output: [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]
Explanation: Surrounded regions should not be on the border, which means that any 'O' on the border of the board are not flipped to 'X'. Any 'O' that is not on the border and it is not connected to an 'O' on the border will be flipped to 'X'. Two cells are connected if they are adjacent cells connected horizontally or vertically.
```
## Classic Solution
The trick is starting DFS from the `O` elements on the border. And any `O` along the way is in regions which is not captured so we mark it as `#`.

After the DFS, the `O`s in the matrix represent the captured regions while `#`s represent the uncaptured regions. So we need to change `O` into `X` and `#` into `O` in the last step.

```C++
class Solution {
public:
    void solve(vector<vector<char>>& board) {
        int m = board.size();
        int n = board[0].size();
        
        for(int j = 0; j < n; j++) {
            if(board[0][j] == 'O')
                dfs(board, 0, j);
            if(board[m-1][j] == 'O')
                dfs(board, m-1, j);
        }
        
        for(int i = 0; i < m; i++) {
            if(board[i][0] == 'O')
                dfs(board, i, 0);
            if(board[i][n-1])
                dfs(board, i, n-1);
        }
        
        for(int i = 0; i < m; i++)
            for(int j = 0; j < n; j++) {
                if(board[i][j] == 'O')
                    board[i][j] = 'X';
                else if(board[i][j] == '#')
                    board[i][j] = 'O';
            }
    }

private:
    void dfs(vector<vector<char>> &board, int x, int y) {
        int m = board.size();
        int n = board[0].size();
        
        if(x < 0 || x >= m || y < 0 || y >= n || board[x][y] != 'O')
            return;
        
        board[x][y] = '#';
        
        dfs(board, x-1, y);
        dfs(board, x+1, y);
        dfs(board, x, y-1);
        dfs(board, x, y+1);
    }
};
```