# 240. Search a 2D Matrix

## Description
Write an efficient algorithm that searches for a value target in an `m x n` integer matrix `matrix`. This matrix has the following properties:

- Integers in each row are sorted in ascending from left to right.
- Integers in each column are sorted in ascending from top to bottom.

**Example:**
```
Input: matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
Output: true
```

## My Solution

I first used two binary search to find the maximum row index and column index, shrinking the size of the search area as much as possible.

Then for each row in the shrinked search area, I used binary search to find the target.

```C++
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    int m = matrix.size();
    int n = matrix[0].size();
    
    int low = 0;
    int high = n - 1;
    while(low < high) {
        int mid = high - ((high - low) >> 1);
        if(matrix[0][mid] > target)
            high = mid - 1;
        else
            low = mid;
    }
    int maxCol = high;
    
    low = 0;
    high = m - 1;
    while(low < high) {
        int mid = high - ((high - low) >> 1);
        if(matrix[mid][0] > target)
            high = mid - 1;
        else
            low = mid;
    }
    int maxRow = high;
    
    for(int i = 0; i <= maxRow; i++) {
        low = 0;
        high = maxCol;
        while(low < high) {
            int mid = low + ((high - low) >> 1);
            if(matrix[i][mid] < target)
                low = mid + 1;
            else
                high = mid;
        }
        
        if(target == matrix[i][high])
            return true;
    }
    return false;
}
```

## Classic Solution
We start search the matrix from top right corner, initialize the current position to top right corner, if the target is greater than the value in current position, then the target can not be in entire row of current position because the row is sorted, if the target is less than the value in current position, then the target can not in the entire column because the column is sorted too. We can rule out one row or one column each time, so the time complexity is `O(m+n)`.

This figure illustrated this solution clearly:
![sol](https://leetcode.com/uploads/files/1488858512318-monosnap-2017-03-06-22-48-17.jpg)

```C++
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    int m = matrix.size();
    int n = matrix[0].size();
    
    int row = 0;
    int col = n - 1;
    while(row < m && col >= 0) {
        if(matrix[row][col] > target) 
            col--;
        else if(matrix[row][col] < target)
            row++;
        else
            return true;
    }
    return false;
}
```