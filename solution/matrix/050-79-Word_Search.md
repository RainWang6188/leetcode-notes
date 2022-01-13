# 79. Word Search

## Description
Given an `m x n` grid of characters `board` and a string `word`, return `true` if `word` exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example:**
```
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true
```
## Classic Solution
This can be solve using classic **Depth First Search**.

Each iteration, we repalce the current element in the board with `'*'` to avoid double counting. After that whole dfs, it should be recover to the original character for dfs on other branches.
```C++
bool exist(vector<vector<char>>& board, string word) {
    for(int i = 0; i < board.size(); i++)
        for(int j = 0; j < board[0].size(); j++)
            if(dfs(board, word, i, j))
                return true;
    return false;
}

bool dfs(vector<vector<char>>& board, string word, int x, int y) {
    int m = board.size();
    int n = board[0].size();
    
    if(word.size() == 0)
        return true;
    if(x < 0 || x >= m || y < 0 || y >= n || board[x][y] != word[0])
        return false;
    
    char c = board[x][y];
    board[x][y] = '*';
    string next_word = word.substr(1);
    bool res = dfs(board, next_word, x-1, y) || dfs(board, next_word, x+1, y) ||
                dfs(board, next_word, x, y-1) || dfs(board, next_word, x, y+1);
    board[x][y] = c;
    return res;
}
```