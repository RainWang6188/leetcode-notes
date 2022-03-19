# 85. Maximal Rectangle

## Description
Given a `rows x cols` binary `matrix` filled with `0`'s and `1`'s, find the largest rectangle containing only `1`'s and return its area.

**Example:**
```
Input: matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
Output: 6
Explanation: The maximal rectangle is shown in the above picture.
```

## Classic Soltion
This problem is very difficult!!

We use three vectors to store the current state:
- `height[i]` records the current number of countinous `'1'` in column `i` (i.e. counts the number of successive `'1'`s above (plus the current one));
- `left[i]` records the left most index `j` which satisfies that for any index `k` from `j` to `i`, `height[k] >= height[i]` (i.e. the boundaries of the rectangle which contains the current point with a height of `height[j]`)
- `right[i]` records the right most index `j` which satifies that for any index `k` from `i` to `j`, `height[k] >= height[i]`;

According to the previous definition, the maximal rectangle area containing `matrix[i][j]` would be `(height[j] * (right[j] - left[j] + 1))`

Then the question is how to update the array of height, left, and right.

- **Update height[]**

    Updating height is easy, since `height[j]` counts the successive `'1'`'s above `matrix[i][j]` inclusive.
    ```C++
        for(int j = 0; j < n; j++) {
            if(matrix[i][j] == '1')
                height[j]++;
            else
                height[j] = 0;
        }
    ```
- **Update left[]**

    Updating `left` and `right` is quite tricky. 

    For `left[]`, we traverse from left to right.
    ```C++
    int lB = 0;  //lB is indicating the left boundry for the current row of the matrix (for cells with char "1"), not the height array...
    for (int j = 0; j < n; j++) {
         if (matrix[i][j] == '1') {
             left[j] = Math.max(left[j], lB); // this means the current boundry should satisfy two conditions: within the boundry of the previous height array, and within the boundry of the current row...
         } else {
             left[j] = 0; // when matrix[i][j] = 0, height[j] will get update to 0, so left[j] becomes 0, since all height in between 0 - j satisfies the condition of height[k] >= height[j];
             lB = j + 1; //and since current position is '0', so the left most boundry for next "1" cell is at least j + 1;
         }
    }
    ```
- **Update right[]**
    And updating `right` is similar, we just need to traverse from right to left.
```C++
int maximalRectangle(vector<vector<char>>& matrix) {
    if(matrix.empty())
        return 0;
    
    int m = matrix.size();
    int n = matrix.front().size();
    
    vector<int> height(n, 0);
    vector<int> left(n, 0);
    vector<int> right(n, n-1);
    
    int maxArea = 0;
    
    for(int i = 0; i < m; i++) {
        // update height[]            
        for(int j = 0; j < n; j++) {
            if(matrix[i][j] == '1')
                height[j]++;
            else
                height[j] = 0;
        }
        
        // update left[]
        int currLeft = 0;
        for(int j = 0; j < n; j++) {
            if(matrix[i][j] == '1') {
                left[j] = max(left[j], currLeft);
            }
            else {
                left[j] = 0;
                currLeft = j + 1;
            }
        }
        
        // update right[]
        int currRight = n - 1;
        for(int j = n -1; j >= 0; j--) {
            if(matrix[i][j] == '1') {
                right[j] = min(right[j], currRight);
                maxArea = max(maxArea, (right[j] - left[j] + 1) * height[j]);
            }
            else {
                right[j] = n - 1;
                currRight = j - 1;
            }
        }
    }
    return maxArea;
}
```

We've separate the loops for comprehension in the above code. We can combine them for better efficiency.

```C++
int maximalRectangle(vector<vector<char>>& matrix) {
    if(matrix.empty())
        return 0;
    
    int m = matrix.size();
    int n = matrix.front().size();
    
    vector<int> height(n, 0);
    vector<int> left(n, 0);
    vector<int> right(n, n-1);
    
    int maxArea = 0;
    
    for(int i = 0; i < m; i++) {
        int currLeft = 0;
        for(int j = 0; j < n; j++) {
            if(matrix[i][j] == '1') {
                height[j]++;
                left[j] = max(left[j], currLeft);
            }
            else {
                height[j] = 0;
                left[j] = 0;
                currLeft = j + 1;
            }
        }
        
        int currRight = n - 1;
        for(int j = n - 1; j >= 0; j--) {
            if(matrix[i][j] == '1') {
                right[j] = min(right[j], currRight);
                maxArea = max(maxArea, (right[j] - left[j] + 1) * height[j]);
            }
            else {
                right[j] = n - 1;
                currRight = j - 1;
            }
        }
    }
    return maxArea;
}
```