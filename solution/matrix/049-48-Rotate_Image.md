# 48. Rotate Image
## Description
You are given an `n x n` 2D matrix representing an image, rotate the image by **90** degrees (clockwise).

You have to rotate the image **in-place**, which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

**Example:**
```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [[7,4,1],[8,5,2],[9,6,3]]
Explanation:
                1 2 3       7 4 1
                4 5 6  ---> 8 5 2
                7 8 9       9 6 3
```
## Classic Solution
### 1. Rotate Group of 4 Cells
We iterate over each group of 4 cells and rotate them. A greate animation can help to realize how it works [here](https://leetcode.com/problems/rotate-image/solution/)

```C++
void rotate(vector<vector<int>>& matrix) {
    int n = matrix.size();
    for(int i = 0; i < (n + 1) / 2; i++) {
        for(int j = 0; j < n / 2; j++) {
            int temp = matrix[n-j-1][i];
            matrix[n-j-1][i] = matrix[n-i-1][n-j-1];
            matrix[n-i-1][n-j-1] = matrix[j][n-i-1];
            matrix[j][n-i-1] = matrix[i][j];
            matrix[i][j] = temp;
        }
    }
}
```

### 2. Rotate Ring by Ring
Another [method](https://leetcode.com/problems/rotate-image/discuss/19002/4ms-few-lines-C%2B%2B-code-Rotate-Image-90-degree-for-O(1)-space) is to rotate ring by ring. The code is as follows
```C++
void rotate(vector<vector<int>>& matrix) {
    int n = matrix.size();
    
    for(int row = 0; row < n/2; row++)
        for(int col = row; col < n - row - 1; col++){                
            swap(matrix[row][col], matrix[col][n - 1 - row]);
            swap(matrix[row][col], matrix[n - 1 - row][n - 1 - col]);
            swap(matrix[row][col], matrix[n - 1 - col][row]);
        }
}
```


Here's an  example:
```
    Input 
    1, 2, 3
    4, 5, 6
    7, 8, 9 


    for-loop 1 
    swap1               swap2               swap3
    1<->3               3<->9              9<->7

    3, 2, 1            9, 2, 1            7, 2, 1
    4, 5, 6    =>      4, 5, 6    =>      4, 5, 6
    7, 8, 9            7, 8, 3            9, 8, 3

    

    for-loop 2 
    swap1              swap2              swap3
    2<->6              6<->8              8<->4

    7, 6, 1            7, 8, 1            7, 4, 1
    4, 5, 2    =>      4, 5, 2    =>      8, 5, 2
    9, 8, 3            9, 6, 3            9, 6, 3
    

    output     
    7, 4, 1
    8, 5, 2
    9, 6, 3
```


### 3. Reverse on Diagonal and Reverse Left to Right
The most elegant solution for rotating the matrix is to firstly **reverse the matrix around the main diagonal**, and then **reverse it from left to right**. These operations are called **transpose** and **reflect** in linear algebra.

The code is easy to comprehension as well as implementation.

```C++
void rotate(vector<vector<int>>& matrix) {
    transpose(matrix);
    reflect(matrix);
}

void transpose(vector<vector<int>>& matrix) {
    int n = matrix.size();
    for(int i = 0; i < n ; i++)
        for(int j = i + 1; j < n; j++) {
            int temp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = temp;
        }
}

void reflect(vector<vector<int>>& matrix) {
    int n = matrix.size();
    for(int i = 0; i < n; i++)
        for(int j = 0; j < n/2; j++) {
            int temp = matrix[i][j];
            matrix[i][j] = matrix[i][n-j-1];
            matrix[i][n-j-1] = temp;
        }
}
```

## A Commen Method for Image Rotation
### 1. Clockwise Rotate
First reverse up to down, and then swap the symmetry (transpose).

```C++
/*
 * clockwise rotate
 * first reverse up to down, then swap the symmetry 
 * 1 2 3     7 8 9     7 4 1
 * 4 5 6  => 4 5 6  => 8 5 2
 * 7 8 9     1 2 3     9 6 3
*/
void rotate(vector<vector<int> > &matrix) {
    reverse(matrix.begin(), matrix.end());
    for (int i = 0; i < matrix.size(); ++i) {
        for (int j = i + 1; j < matrix[i].size(); ++j)
            swap(matrix[i][j], matrix[j][i]);
    }
}
```

### 2. Anticlockwise Rotate
First reverse up to right, and then swap the symmetry (transpose).
```C++
/*
 * anticlockwise rotate
 * first reverse left to right, then swap the symmetry
 * 1 2 3     3 2 1     3 6 9
 * 4 5 6  => 6 5 4  => 2 5 8
 * 7 8 9     9 8 7     1 4 7
*/
void anti_rotate(vector<vector<int> > &matrix) {
    for (auto vi : matrix) reverse(vi.begin(), vi.end());
    for (int i = 0; i < matrix.size(); ++i) {
        for (int j = i + 1; j < matrix[i].size(); ++j)
            swap(matrix[i][j], matrix[j][i]);
    }
}
```