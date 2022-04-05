# 302. Smallest Rectangle Enclosing Black Pixels

## Description
Description
An image is represented by a binary matrix with 0 as a white pixel and 1 as a black pixel. The black pixels are connected, i.e., there is only one black region. Pixels are connected horizontally and vertically. Given the location (x, y) of one of the black pixels, return the area of the smallest (axis-aligned) rectangle that encloses all black pixels.

**Example:**
```
Input：["0010","0110","0100"]，x=0，y=2
Output：6
Explanation：
The upper left coordinate of the matrix is (0,1), and the lower right coordinate is (2,2).
```

## My Solution

A standard BFS solution, finding the minimum and maximum index horizontally and vertically.

```C++
int minArea(vector<vector<char>> &image, int x, int y) {
    if(image.empty() || image[x][y] != '1')
        return 0;

    int m = image.size();
    int n = image[0].size();
    int minX = x;
    int maxX = x;
    int minY = y;
    int maxY = y;
    
    vector<vector<int>> dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    queue<pair<int, int>> q;
    q.push(make_pair(x, y));
    while(!q.empty()) {
        int currX = q.front().first;
        int currY = q.front().second;
        q.pop();

        minX = min(currX, minX);
        maxX = max(currX, maxX);
        minY = min(currY, minY);
        maxY = max(currY, maxY);
        image[currX][currY] = '#';

        for(auto& dir : dirs) {
            int nextX = currX + dir[0];
            int nextY = currY + dir[1];
            if(nextX < 0 || nextX >= m || nextY < 0 || nextY >= n || image[nextX][nextY] != '1')
                continue;
            q.push(make_pair(nextX, nextY));
        }
    }
    return (maxX - minX + 1) * (maxY - minY + 1);
}
```

## Classic Solution
The above BFS solution is not very efficient when there're many black pixels in the `image`. A much better way under that circumstance is binary search.


```C++
class Solution {
public:
    int minArea(vector<vector<char>> &image, int x, int y) {
        if(image.empty() || image[x][y] != '1')
            return 0;

        int m = image.size();
        int n = image[0].size();

        int minX = x;
        int maxX = x;
        int minY = y;
        int maxY = y;

        int low = 0;
        int high = x;
        while(low < high) {
            int mid = low + ((high - low) >> 1);
            if(checkRow(image, mid))
                high = mid;
            else
                low = mid + 1;
        }
        minX = high;

        low = x;
        high = m - 1;
        while(low < high) {
            int mid = high - ((high - low) >> 1);
            if(checkRow(image, mid))
                low = mid;
            else
                high = mid - 1;
        }
        maxX = high;

        low = 0;
        high = y;
        while(low < high) {
            int mid = low + ((high - low) >> 1);
            if(checkCol(image, mid))
                high = mid;
            else
                low = mid + 1;
        }
        minY = high;

        low = y;
        high = n - 1;
        while(low < high) {
            int mid = high - ((high - low) >> 1);
            if(checkCol(image, mid))
                low = mid;
            else
                high = mid - 1;
        }
        maxY = high;

        return (maxX - minX + 1) * (maxY - minY + 1);
    }
private:
    bool checkRow(const vector<vector<char>> &image, const int row) {
        for(int j = 0; j < image[0].size(); j++)
            if(image[row][j] == '1')
                return true;
        return false;
    }

    bool checkCol(const vector<vector<char>> &image, const int col) {
        for(int i = 0; i < image.size(); i++)
            if(image[i][col] == '1')
                return true;
        return false;
    }
};
```