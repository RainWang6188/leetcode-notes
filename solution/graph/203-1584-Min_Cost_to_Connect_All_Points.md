# 203. Min Cost to Connect All Points

## Description

You are given an array `points` representing integer coordinates of some points on a 2D-plane, where `points[i] = [xi, yi]`.

The cost of connecting two `points` `[xi, yi]` and `[xj, yj]` is the manhattan distance between them: `|xi - xj| + |yi - yj|`, where `|val|` denotes the absolute value of `val`.

Return the minimum cost to make all points connected. All points are connected if there is **exactly one** simple path between any two points.

**Example:**

![q](https://assets.leetcode.com/uploads/2020/08/26/d.png)
```
Input: points = [[0,0],[2,2],[3,10],[5,2],[7,0]]
Output: 20
Explanation: 

We can connect the points as shown following to get the minimum cost of 20.

Notice that there is a unique path between every pair of points.
```

![sol](https://assets.leetcode.com/uploads/2020/08/26/c.png)

## Classic Solution

We can imagine that every point is a node of the graph, connected to all other points, and the lenght of the edge is the manhattan distance between two points.

To find the min cost, we therefore need to find the minimum spanning tree.

### 1. Prim's Algorithm

In the Prim's algorithm, we are building a tree starting from some initial point. We track all connected points in visited. 

For the current point, we add its edges to the min heap. Then, we pick a smallest edge that connects to a point that is not visited. Repeat till all points are visited.


```C++
int minCostConnectPoints(vector<vector<int>>& points) {
    int index = 0;
    int cost = 0;
    vector<bool> visited(points.size(), false);
    auto cmp = [](pair<int, int>& p1, pair<int, int>& p2) {
        return p1.second > p2.second;  
    };
    priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> pq(cmp);
    
    for(int i = 1; i < points.size(); i++) {
        visited[index] = true;
        for(int j = 0; j < points.size(); j++) {
            if(!visited[j])
                pq.push(make_pair(j, abs(points[index][0] - points[j][0]) + abs(points[index][1] - points[j][1])));
        }
        
        while(visited[pq.top().first])
            pq.pop();
        index = pq.top().first;
        cost += pq.top().second;
        pq.pop();
    }
    return cost;
}
```

### 2. Kruskal Algorithm

This solution can be viewed [here](https://leetcode.com/problems/min-cost-to-connect-all-points/discuss/843940/C%2B%2B-MST%3A-Kruskal-%2B-Prim's-%2B-Complete-Graph).