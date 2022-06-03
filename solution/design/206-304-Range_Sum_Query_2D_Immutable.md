# 304. Range Sum Query 2D - Immutable

## Description

Given a 2D matrix `matrix`, handle multiple queries of the following type:

- Calculate the **sum** of the elements of matrix inside the rectangle defined by its upper left corner `(row1, col1)` and lower right corner `(row2, col2)`.

Implement the `NumMatrix` class:

- `NumMatrix(int[][] matrix)` Initializes the object with the integer matrix `matrix`.
- `int sumRegion(int row1, int col1, int row2, int col2)` Returns the sum of the elements of matrix inside the rectangle defined by its upper left corner `(row1, col1)` and lower right corner `(row2, col2)`.


## My Solution

Brute Force, which will exceed the time limit.

```C++
class NumMatrix {
private:
    vector<vector<int>> matrix;
public:
    NumMatrix(vector<vector<int>>& matrix) {
        this->matrix = matrix;
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        int sum = 0;
        for(int i = row1; i <= row2; i++)
            for(int j = col1; j <= col2; j++)
                sum += (this->matrix)[i][j];
        return sum;
    }
    
    ~NumMatrix() {
        matrix.clear();
    }
};
```

## Classic Solution

Here's a solution which takes `O(mn)` space and `O(1)` time by [haoel](https://leetcode.com/problems/range-sum-query-2d-immutable/discuss/75350/Clean-C%2B%2B-Solution-and-Explaination-O(mn)-space-with-O(1)-time).


```C++
class NumMatrix {
private:
    vector<vector<int>> sums;
public:
    NumMatrix(vector<vector<int>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        sums = vector<vector<int>>(m + 1, vector<int>(n + 1, 0));
        
        for(int i = 1; i <= m; i++) {
            for(int j = 1; j <= n; j++) {
                sums[i][j] = sums[i-1][j] + sums[i][j-1] + matrix[i-1][j-1] - sums[i-1][j-1];
            }
        }
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        return sums[row2+1][col2+1] - sums[row2+1][col1] - sums[row1][col2+1] + sums[row1][col1];
    }
    
    ~NumMatrix() {
        sums.clear();
    }
};
```