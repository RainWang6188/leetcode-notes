# 311. Sparse Matrix Multiplication

## Description
Given two sparse matrices `A` and `B`, return the result of `AB`.

You may assume that `A`'s column number is equal to `B`'s row number.

**Example:**
```
A = [
  [ 1, 0, 0],
  [-1, 0, 3]
]

B = [
  [ 7, 0, 0 ],
  [ 0, 0, 0 ],
  [ 0, 0, 1 ]
]


     |  1 0 0 |   | 7 0 0 |   |  7 0 0 |
AB = | -1 0 3 | x | 0 0 0 | = | -7 0 3 |
                  | 0 0 1 |
```
## Classic Solution
There're two ways to perform matrix multiplication:

Suppose `A(n, t) * B(t, m) = C(n, m)`

- Method 1: Traverse each value of `C[i][j]` in the outer loop
```C++
for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        for (int k = 0; k < t; k++) {
            C[i][j] += A[i][k] * B[k][j];
        }
    }
}
```
- Method 2: Traverse each value of `A[i][j]` in the outer loop
```C++
for (int i = 0; i < n; i++) {
    for (int k = 0; k < t; k++) {
        for (int j = 0; j < m; j++) {
            C[i][j] += A[i][k] * B[k][j];
        }
    }
}
```

Since the matrix is sparse, we can traverse matrix `A` first (as method 2) instead of computing `C[i][j]` directly (as method 1). If `A[i][j] == 0`, then we can directly skip further computation.

As for matrix `B`, we can first traverse it and store the column index of non-zero elements, and then perform multiplication.

```C++
vector<vector<int>> multiply(vector<vector<int>> &A, vector<vector<int>> &B) {
    // write your code here
    int m = A.size();
    int t = B.size();
    int n = B.front().size();

    vector<vector<int>> C(m, vector<int>(n, 0));
    vector<vector<int>> BNotZeroCol(t);
    for(int i = 0; i < t; i++)
        for(int j = 0; j < n; j++)
            if(B[i][j])
                BNotZeroCol[i].push_back(j);
    
    for(int i = 0; i < m; i++) {
        for(int j = 0; j < t; j++) {
            if(!A[i][j])
                continue;
            
            for(auto& col : BNotZeroCol[j])
                C[i][col] += A[i][j] * B[j][col];
        }
    }
    return C;
}
```