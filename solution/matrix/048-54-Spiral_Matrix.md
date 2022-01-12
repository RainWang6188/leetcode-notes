# 54. Spiral Matrix

## Description
Given an `m x n` matrix, return all elements of the matrix in spiral order.

**Example:**
```
Input: matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```
## My Solution
I modify the elements in the matrix after searching to make it easier for boundary determination. Now the boundary can be defined as index out of bound or the next element has been searched before.

Here's the procedure:

1. Search rightwards and mark the element as `computed` until we reach the boundary.
    
2. Search downwards and mark the element as `computed` until we reach the boundary.

3. Search leftwards and mark the element as `computed` until we reach the boundary.

4. Search upwards and mark the element as `computed` until we reach the boundary.

After searching of each direction, we will check if the first element in the next direction is valid, if not, we will terminate.

```C++
vector<int> spiralOrder(vector<vector<int>>& matrix) {
    vector<int> elements;
    int m = matrix.size();
    int n = matrix[0].size();
    
    const int computed = -200;
    int i = 0;
    int j = 0;
    while(1) {
        while(j < n && matrix[i][j] != computed) {
            elements.push_back(matrix[i][j]);
            matrix[i][j] = computed;
            j++;
        } 
        j--;
        if(i + 1 >= m || matrix[i + 1][j] == computed)
            break;
        i++;
        
        while(i < m && matrix[i][j] != computed) {
            elements.push_back(matrix[i][j]);
            matrix[i][j] = computed;
            i++;
        } 
        i--;
        if(j - 1 < 0 || matrix[i][j - 1] == computed)
            break;
        j--;
        
        while(j >= 0 && matrix[i][j] != computed) {
            elements.push_back(matrix[i][j]);
            matrix[i][j] = computed;
            j--;
        }
        j++;
        if(i - 1 < 0 || matrix[i - 1][j] == computed)
            break;
            i--;
        
        while(i >= 0 && matrix[i][j] != computed) {
            elements.push_back(matrix[i][j]);
            matrix[i][j] = computed;
            i--;
        }
        i++;
        if(j + 1 >= n || matrix[i][j + 1] == computed)
            break;
        j++;
    }
    return elements;
}
```
## Classic Solution
A more intuitive solution uses four variables `row_begin`, `row_end`, `col_begin` and `col_end` to control the border.

Note that here we need to guarantee that `res.size() < m * n` to prevent duplicates.

```C++
vector<int> spiralOrder(vector<vector<int>>& matrix) {
    vector<int> res;
    int m = matrix.size();
    int n = matrix[0].size();
    
    int col_begin = 0;
    int col_end = n - 1;
    int row_begin = 0;
    int row_end = m - 1;
    
    while(res.size() < m * n) {
        for(int j = col_begin; j <= col_end && res.size() < m * n; j++)
            res.push_back(matrix[row_begin][j]);
        row_begin++;
        
        for(int i = row_begin; i <= row_end && res.size() < m * n; i++)
            res.push_back(matrix[i][col_end]);
        col_end--;
        
        for(int j = col_end; j >= col_begin && res.size() < m * n; j--)
            res.push_back(matrix[row_end][j]);
        row_end--;
        
        for(int i = row_end; i >= row_begin && res.size() < m * n; i--)
            res.push_back(matrix[i][col_begin]);
        col_begin++;
    }
    return res;
}
```