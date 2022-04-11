# 218. The Skyline Problem


## Description

A city's skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Given the locations and heights of all the buildings, return the skyline formed by these buildings collectively.

The geometric information of each building is given in the array `buildings` where `buildings[i] = [lefti, righti, heighti]`:

- `lefti` is the x coordinate of the left edge of the `ith` building.
- `righti` is the x coordinate of the right edge of the `ith` building.
- `heighti` is the height of the `ith` building.

You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height `0`.

The skyline should be represented as a list of "key points" sorted by their x-coordinate in the form `[[x1,y1],[x2,y2],...]`. Each key point is the left endpoint of some horizontal segment in the skyline except the last point in the list, which always has a y-coordinate `0` and is used to mark the skyline's termination where the rightmost building ends. Any ground between the leftmost and rightmost buildings should be part of the skyline's contour.

Note: There must be no consecutive horizontal lines of equal height in the output skyline. For instance, `[...,[2 3],[4 5],[7 5],[11 5],[12 7],...]` is not acceptable; the three lines of height 5 should be merged into one in the final output as such: `[...,[2 3],[4 5],[12 7],...]`

## Classic Solution

This problem is basically a scan line problem. We scan from the left to right according to the x-coordinate while keeping track of the current maximum height, adding the top left points of the contour to the result.

We can use priority queue to store the current heights. But owing to the fact that we need to erase some heights during the process and priority queue doesn't provide a deletion in the C++ STL, so we turn to `std::multiset` as a replacement. But for the clarity as well as comprehension, I will use the priority queue in the following explanation.

Here's a algorithm:

- First, we sort the top two vertices of all the rectangles so that they're ascending in x-coordinate. We choose the set the height of the starting point as `-h` while the height of ending point as `h` to satisfy the following requirements:
    - The x-coordinate must be in non-descending order
    - If x position are equal, the starting point should be in front of the ending point
    - If the x-coordinates are equal, and both are **starting points**, then they should be in **descending** order of their heights
    - If the x-coordinate are equal, and both are **ending points**, then they should be in **ascending** order of their heights.

- We insert `0` to the initial empty priority queue since right bottom points of the contour should be considered.
![why insert 0](https://pic.leetcode-cn.com/1626177990-hmndOf-image.png)

- We use a variable `prev` to keep track of the previous maximum height, so that we only update the result if the current maximum height is different from the previous maximum height. (Merge the consecutive horizontal lines)

- Then, we scaning the sorted points from left to right.
    - If the current point is a starting point, we insert the corresponding height to the priority queue.
    - If the current point is an ending point, we delete the corresponding height from the priority queue.
    
    - Then we get the current maximum height from the top of the priority queue. Updat the result if the current maixmum height is different from the previous maximum height. Don't forget to update the `prev` as well.


```C++
vector<vector<int>> getSkyline(vector<vector<int>>& buildings) {
    vector<vector<int>> sortedBuildings;
    vector<vector<int>> res;
    for(auto& building : buildings) {
        sortedBuildings.push_back({building[0], -building[2]});
        sortedBuildings.push_back({building[1], building[2]});
    }
    auto cmp = [](auto& point1, auto& point2) {
        if(point1[0] != point2[0])  
            return point1[0] < point2[0];
        else
            return point1[1] < point2[1];
    };
    sort(sortedBuildings.begin(), sortedBuildings.end(), cmp);
    
    multiset<int, greater<int>> pq;
    pq.insert(0);
    int prev = 0;
    for(auto& p : sortedBuildings) {
        int xPos = p[0];
        int height = p[1];
        
        if(height < 0)
            pq.insert(-height);
        else
            pq.erase(pq.find(height));
        
        int curr = *(pq.begin());
        if(curr != prev) {
            res.push_back({xPos, curr});
            prev = curr;
        }
    }
    return res;
}
```