# 118. Pascal's Triangle
## Description
Given an integer `numRows`, return the first numRows of **Pascal's triangle**.

In **Pascal's triangle**, each number is the sum of the two numbers directly above it.

**Example**
```
Input: numRows = 5
Output: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```
## My Solution

### 1. Iterative Version
- For `numRows == 1 or 2`, we can simply insert the constant rows.
- For `numRows > 2`
    Simply extract the last row and insert the sum of the two numbers directly above it.
```C++
vector<vector<int>> generate(int numRows) {
    vector<vector<int>> res;
    vector<int> first_row = {1};
    vector<int> second_row = {1, 1};
    
    for(int i = 0; i < numRows; i++) {
        if(i == 0)
            res.push_back(first_row);
        else if(i == 1)
            res.push_back(second_row);
        else {
            vector<int> last_row = res[res.size()-1];
            vector<int> new_row = {1};
            for(int i = 0; i < last_row.size() - 1; i++) {
                new_row.push_back(last_row[i] + last_row[i+1]);
            }
            new_row.push_back(1);
            
            res.push_back(new_row);
        }
    }
    
    return res;
}
```

### 2. Recursive Version
We can convert the above algorithm to recursive version as follows:
``` C++
vector<vector<int>> generate(int numRows) {
    vector<vector<int>> res;
    vector<int> first_row = {1};
    vector<int> second_row = {1, 1};
    
    if(numRows == 1) {
        res.push_back(first_row);
        return res;
    }
    else if(numRows == 2) {
        res.push_back(first_row);
        res.push_back(second_row);
        return res;
    }
    else {
        vector<vector<int>> last_pascal = generate(numRows - 1);
        vector<int> last_row = last_pascal[last_pascal.size()-1];
        vector<int> new_row = {1};
        for(int i = 0; i < last_row.size() - 1; i++)
            new_row.push_back(last_row[i] + last_row[i+1]);
        new_row.push_back(1);
        
        res = last_pascal;
        res.push_back(new_row);
    }
        
    
    return res;
}
```

## Classic Solution
Take it the vector as two dimensional array and fully employ the `resize()` property of vectors.
```C++
vector<vector<int>> generate(int numRows) {
    vector<vector<int>> r(numRows);

    for (int i = 0; i < numRows; i++) {
        r[i].resize(i + 1);
        r[i][0] = r[i][i] = 1;

        for (int j = 1; j < i; j++)
            r[i][j] = r[i - 1][j - 1] + r[i - 1][j];
    }
    
    return r;
}
```