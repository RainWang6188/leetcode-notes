# 36. Valid Sudoku

## Description
Determine if a `9 x 9` Sudoku `board` is valid. Only the filled cells need to be validated **according to the following rules**:

- Each row must contain the digits `1-9` without repetition.
- Each column must contain the digits `1-9` without repetition.
- Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without repetition.

**Note:**

- A Sudoku board (partially filled) could be valid but is not necessarily solvable.
- Only the filled cells need to be validated according to the mentioned rules.

**Example:**
```
Input: board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: true
```

## My Solution
A standard solution to check if the 3 requirements in the description hold.

```C++
bool isValidSudoku(vector<vector<char>>& board) {
    if(board.empty())
        return true;
    
    int len = 9;
    vector<vector<int>> row(len, vector<int>(9, 0));
    vector<vector<int>> col(len, vector<int>(9, 0));
    for(int i = 0; i < len; i++) {
        for(int j = 0; j < len; j++) {
            if(board[i][j] >= '1' && board[i][j] <= '9') {
                int index = board[i][j] - '1';
                if(++row[i][index] > 1)
                    return false;
                if(++col[j][index] > 1)
                    return false;
            }
        }
    }
    
    int x = 0;
    int y = 0;
    vector<vector<int>> nextPos = {{0, 0}, {0, 3}, {0, 6}, {3, 0}, {3, 3}, {3, 6}, {6, 0}, {6, 3}, {6, 6}};
    for(auto& pos : nextPos) {
        int currX = x + pos[0];
        int currY = y + pos[1];
        vector<int> count(9, 0);
        for(int i = 0; i < 3; i++) {
            for(int j = 0; j < 3; j++) {
                if(board[currX+i][currY+j] >= '1' && board[currX+i][currY+j] <= '9') {
                    int index = board[currX+i][currY+j] - '1';
                    if(++count[index] > 1)
                        return false;
                }       
            }
        }
    }
    return true;
}
```

## Classic Solution
Here's an elegant solution using just one hash set.

```C++
bool isValidSudoku(vector<vector<char>>& board) {
    if(board.empty())
        return true;
    
    unordered_set<string> mySet;
    for(int i = 0; i < 9; i++) {
        for(int j = 0; j < 9; j++) {
            char curr = board[i][j];
            if(curr != '.') {
                auto pr1 = mySet.insert(to_string(curr) + " in row " + to_string(i));
                auto pr2 = mySet.insert(to_string(curr) + " in col " + to_string(j));
                auto pr3 = mySet.insert(to_string(curr) + " in block " + to_string(i/3) + "-" + to_string(j/3));
                
                if(!pr1.second || !pr2.second || !pr3.second)
                    return false;
            }
        }
    }
    return true;
}
```