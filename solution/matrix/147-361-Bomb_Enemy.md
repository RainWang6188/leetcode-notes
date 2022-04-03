# 361. Bomb Enemy

## Description

Given a 2D grid, each cell is either a wall `'W'`, an enemy `'E'` or empty `'0'` (the number zero), return the maximum enemies you can kill using one bomb.

The bomb kills all the enemies in the same row and column from the planted point until it hits the wall since the wall is too strong to be destroyed.

**Example:**
```
Input:
grid =[
     "0E00",
     "E0WE",
     "0E00"
]
Output: 3
Explanation:
Placing a bomb at (1,1) kills 3 enemies
```

## Classic Solution

First naive approach: just go through each location. If it’s `'0'`, count `'E'` in current row and column between `'W'`. If it’s `'E'`, or W, do nothing and move forward.

The problem here is: while we scan each element in the same row, we calculate how many `'E'` in this row n times (assume n is the number of columns). To save time, we simply store the calculation in a variable every time we scan the first element in a new row. But be careful about the `'W'`. After we hit the `'W'`, we need to recount `'E'` and update the variable.

The same approach applies to each column. Since we are scanning through each row, we jump from one column to another column. So we need an array to store `'E'` counts for each column.

With those two auxiliary storages, we can just update the max `'E'` counts when we encounter `'0'` every time.

```C++
int maxKilledEnemies(vector<vector<char>> &grid) {
    if(grid.empty())
        return 0;

    int maxCount = 0;
    int m = grid.size();
    int n = grid[0].size();
    int rowCount = 0;
    vector<int> colCount(n, 0);

    for(int i = 0; i < m; i++) {
        for(int j = 0; j < n; j++) {
            if(!j || grid[i][j-1] == 'W') {
                rowCount = 0;
                for(int k = j; k < n && grid[i][k] != 'W'; k++)
                    if(grid[i][k] == 'E')
                        rowCount++;
            }

            if(!i || grid[i-1][j] == 'W') {
                colCount[j] = 0;
                for(int k = i; k < m && grid[k][j] != 'W'; k++)
                    if(grid[k][j] == 'E')
                        colCount[j]++;
            }
            
            if(grid[i][j] == '0')
                maxCount = max(maxCount, rowCount + colCount[j]);
        }
    }
    return maxCount;
}
```