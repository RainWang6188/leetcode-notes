# 51. N-Queens

## Description
The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return all distinct solutions to the **n-queens puzzle**. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

**Example:**
```
Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
Explanation: There exist two distinct solutions to the 4-queens puzzle
```

## My Solution
My solution is a standard DFS.

At each iteration, we check the validity of placing a Queen at current position. If it's valid, we'll try to place a new Queen in the next row.

```C++
class Solution {
public:
    vector<vector<string>> solveNQueens(int n) {
        vector<string> board(n, string(n, '.'));
        
        for(int j = 0; j < n; j++) {
            dfs(board, 0, j);
        }
        
        return res;
    }

private:
    vector<vector<string>> res = {};
    
    bool willAttack(const vector<string> &board, const int x, const int y) {
        int n = board.size();
        
        //check horizontal
        for(int j = 0; j < n; j++) {
            if(j == y)
                continue;
            else if(board[x][j] == 'Q')
                return true;
        }
        
        // check vertical
        for(int i = 0; i < n; i++) {
            if(i == x)
                continue;
            else if(board[i][y] == 'Q')
                return true;
        }
        
        // check diagonal
        for(int i = 1; i < n && (x - i) >= 0 && (y - i) >= 0; i++)
            if(board[x-i][y-i] == 'Q')
                return true;
        
        for(int i = 1; i < n && (x + i) < n && (y + i) < n; i++)
            if(board[x+i][y+i] == 'Q')
                return true;
        
        for(int i = 1; i < n && (x - i) >= 0 && (y + i) < n; i++)
            if(board[x-i][y+i] == 'Q')
                return true;
        
        for(int i = 1; i < n && (x + i) < n && (y - i) >= 0; i++)
            if(board[x+i][y-i] == 'Q')
                return true;
        
        return false;
    }
    
    void dfs(vector<string> &board, int x, int y) {
        int n = board.size();
        
        if(x < 0 || x >= n || y < 0 || y >= n || willAttack(board, x, y))
            return;
        
        board[x][y] = 'Q';
        
        if(x == n-1) {
            res.push_back(board);
        }
        else {
            for(int j = 0; j < n; j++)
                dfs(board, x+1, j);
        }
        
        board[x][y] = '.';
    }
};
```
Actually we don't need to perform DFS position by position. We can simply do it row by row. The following code is much concise:
```C++
class Solution {
public:
    std::vector<std::vector<std::string> > solveNQueens(int n) {
        std::vector<std::vector<std::string> > res;
        std::vector<std::string> nQueens(n, std::string(n, '.'));
        solveNQueens(res, nQueens, 0, n);
        return res;
    }
private:
    void solveNQueens(std::vector<std::vector<std::string> > &res, std::vector<std::string> &nQueens, int row, int &n) {
        if (row == n) {
            res.push_back(nQueens);
            return;
        }
        for (int col = 0; col != n; ++col)
            if (isValid(nQueens, row, col, n)) {
                nQueens[row][col] = 'Q';
                solveNQueens(res, nQueens, row + 1, n);
                nQueens[row][col] = '.';
            }
    }
    bool isValid(std::vector<std::string> &nQueens, int row, int col, int &n) {
        //check if the column had a queen before.
        for (int i = 0; i != row; ++i)
            if (nQueens[i][col] == 'Q')
                return false;
        //check if the 45° diagonal had a queen before.
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; --i, --j)
            if (nQueens[i][j] == 'Q')
                return false;
        //check if the 135° diagonal had a queen before.
        for (int i = row - 1, j = col + 1; i >= 0 && j < n; --i, ++j)
            if (nQueens[i][j] == 'Q')
                return false;
        return true;
    }
};    
```
## Classic Solution

Use flag vectors as bitmask:

The number of columns is `n`, the number of 45° diagonals is `2 * n - 1`, the number of 135° diagonals is also `2 * n - 1`. When reach `[row, col]`, the column No. is `col`, the 45° diagonal No. is `row + col` and the 135° diagonal No. is `n - 1 + col - row`. We can use three arrays to indicate if the column or the diagonal had a queen before, if not, we can put a queen in this position and continue.

**NOTE:** Please don't use `vector<bool>` flag to replace `vector<int>` flag in the following C++ code. In fact, `vector<bool>` is not a STL container. You should avoid to use it. You can also get the knowledge from [here](https://stackoverflow.com/questions/17794569/why-isnt-vectorbool-a-stl-container) and [here](http://stackoverflow.com/questions/670308/alternative-to-vectorbool).
```C++
/**    | | |                / / /             \ \ \
  *    O O O               O O O               O O O
  *    | | |              / / / /             \ \ \ \
  *    O O O               O O O               O O O
  *    | | |              / / / /             \ \ \ \ 
  *    O O O               O O O               O O O
  *    | | |              / / /                 \ \ \
  *   3 columns        5 45° diagonals     5 135° diagonals    (when n is 3)
  */
class Solution {
public:
    std::vector<std::vector<std::string> > solveNQueens(int n) {
        std::vector<std::vector<std::string> > res;
        std::vector<std::string> nQueens(n, std::string(n, '.'));
        std::vector<int> flag_col(n, 1), flag_45(2 * n - 1, 1), flag_135(2 * n - 1, 1);
        solveNQueens(res, nQueens, flag_col, flag_45, flag_135, 0, n);
        return res;
    }
private:
    void solveNQueens(std::vector<std::vector<std::string> > &res, std::vector<std::string> &nQueens, std::vector<int> &flag_col, std::vector<int> &flag_45, std::vector<int> &flag_135, int row, int &n) {
        if (row == n) {
            res.push_back(nQueens);
            return;
        }
        for (int col = 0; col != n; ++col)
            if (flag_col[col] && flag_45[row + col] && flag_135[n - 1 + col - row]) {
                flag_col[col] = flag_45[row + col] = flag_135[n - 1 + col - row] = 0;
                nQueens[row][col] = 'Q';
                solveNQueens(res, nQueens, flag_col, flag_45, flag_135, row + 1, n);
                nQueens[row][col] = '.';
                flag_col[col] = flag_45[row + col] = flag_135[n - 1 + col - row] = 1;
            }
    }
};
```
### Optimization
But we actually do not need to use three arrays, we just need one. Now, when reach `[row, col]`, the subscript of column is `col`, the subscript of 45° diagonal is `n + row + col` and the subscript of 135° diagonal is `n + 2 * n - 1 + n - 1 + col - row`.
```C++
class Solution {
public:
    std::vector<std::vector<std::string> > solveNQueens(int n) {
        std::vector<std::vector<std::string> > res;
        std::vector<std::string> nQueens(n, std::string(n, '.'));
        /*
        flag[0] to flag[n - 1] to indicate if the column had a queen before.
        flag[n] to flag[3 * n - 2] to indicate if the 45° diagonal had a queen before.
        flag[3 * n - 1] to flag[5 * n - 3] to indicate if the 135° diagonal had a queen before.
        */
        std::vector<int> flag(5 * n - 2, 1);
        solveNQueens(res, nQueens, flag, 0, n);
        return res;
    }
private:
    void solveNQueens(std::vector<std::vector<std::string> > &res, std::vector<std::string> &nQueens, std::vector<int> &flag, int row, int &n) {
        if (row == n) {
            res.push_back(nQueens);
            return;
        }
        for (int col = 0; col != n; ++col)
            if (flag[col] && flag[n + row + col] && flag[4 * n - 2 + col - row]) {
                flag[col] = flag[n + row + col] = flag[4 * n - 2 + col - row] = 0;
                nQueens[row][col] = 'Q';
                solveNQueens(res, nQueens, flag, row + 1, n);
                nQueens[row][col] = '.';
                flag[col] = flag[n + row + col] = flag[4 * n - 2 + col - row] = 1;
            }
    }
};
```