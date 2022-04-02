# 141. Search a 2D Matrix

## Description

## My Solution
According to the description, the row as well as the column is strictly increasing, so I used two binary searches to find the target.

The first binary search aims to find the possible row where the target might be. Since we want to find the last element whose value is less than or equal to the target, so a trick here is to use `mid = high - ((high - low) >> 1)` to compute the middle elemenet.

The second binary search aims the find the target in that possible row.
```C++
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    int m = matrix.size();
    int n = matrix[0].size();
    
    int low = 0;
    int high = m - 1;
    while(low < high) {
        int mid = high - ((high - low) >> 1);
        
        if(matrix[mid][0] <= target)
            low = mid;
        else
            high = mid - 1;
    }
    
    int rowIndex = low;
    low = 0;
    high = n - 1;
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        
        if(matrix[rowIndex][mid] >= target)
            high = mid;
        else
            low = mid  +1;
    }
    return matrix[rowIndex][high] == target;
}
```

## Classic Solution
Sincet the whole matrix is strictly increasing, we can treat it as a 1D-array instead of a 2D-matrix.

As a result, we can use just one binary search to find the target rather than first find the row index and next find the column index.

- Suppose converting an array `arr[]` into an m * n matrix `matrix[][]`, then we have `arr[k] => matrix[k/n][k%n]`
- Suppose converting an m * n matrix `matrix[][]` into an array `arr[]`, then we have `matrix[i][j] => arr[i*n+j]`.

However, the one concern here is that store the `m * n` into an integer may cause integer overflow if the size of the matrix is rather large.

```C++
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    int m = matrix.size();
    int n = matrix[0].size();
    
    int low = 0;
    int high = m * n - 1;
    
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        
        if(matrix[mid/n][mid%n] < target)
            low = mid + 1;
        else
            high = mid;
    }
    return matrix[high/n][high%n] == target;
}
```