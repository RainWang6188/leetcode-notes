# 73. Set Matrix Zeroes
## Description
Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s, and return the matrix.

You must do it **in place**.

**Examples:**
```
Input: matrix = [[1,1,1],[1,0,1],[1,1,1]]
Output: [[1,0,1],[0,0,0],[1,0,1]]
```
## My Solution
Acutally my solution is not in-place.

I first find the positions of all the zeroes in the matrix and store the coordindate in a vector.

Next, I traverse the vector and set the elements on the columns and rows of all the positions in the vector.


Also, we can store the rows and cols in `unordered_set` seperately to avoid redundancy.

```C++
void setZeroes(vector<vector<int>>& matrix) {
    vector<pair<int, int>> position;
    
    for(int i = 0; i < matrix.size(); i++)
        for(int j = 0; j < matrix[i].size(); j++)
            if(matrix[i][j] == 0)
                position.push_back(make_pair(i, j));
    
    for(auto pos : position) {
        int row = pos.first;
        int col = pos.second;
        
        for(int j = 0; j < matrix[0].size(); j++)
            matrix[row][j] = 0;
        for(int i = 0; i < matrix.size(); i++)
            matrix[i][col] = 0;
    }
}
```

## Classic Solution
To use $O(1)$ space, we cannot use extra data structure and should fully exploit the original matrix to store the information.

A classic solution stores the state of each row `i` in first element of that row `matrix[i][0]`, and store the state of each column `j` in first element of that column `matrix[0][j]`. In this case, `matrix[0][0]` store not only the state of $row_0$ but also the state of $col_0$, so we use another variable `col_0` to store the state of $col_0$ instead.

This algorithm uses two passes. In the first pass, we set the state of each rows and columns by a top-down scanning. In the second pass, we set the elements' value according to the state by a bottom-up scanning.

The reason why in the second pass we use bottom-up traversal instead of top-down traversal is that using top-down scanning results in the elements in the first row gets changed, influencing the columns in the scanning afterwards.
```C++
void setZeroes(vector<vector<int>>& matrix) {
    int col_0 = 1;
    int rows = matrix.size();
    int cols = matrix[0].size();
    
    for(int i = 0; i < rows; i++) {
        if(matrix[i][0] == 0)
            col_0 = 0;
        
        for(int j = 1; j < cols; j++) {
            if(matrix[i][j] == 0) {
                matrix[i][0] = 0;
                matrix[0][j] = 0;
            }
        }
    }
    
    for(int i = rows - 1; i >= 0; i--) {
        for(int j = cols - 1; j > 0; j--) {
            if(matrix[i][0] == 0 || matrix[0][j] == 0)
                matrix[i][j] = 0;
        }
        if(col_0 == 0)
            matrix[i][0] = 0;
    }
}
```