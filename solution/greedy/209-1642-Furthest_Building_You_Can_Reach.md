# 1642. Furthest Building You Can Reach

## Description

You are given an integer array `heights` representing the heights of buildings, some `bricks`, and some `ladders`.

You start your journey from building `0` and move to the next building by possibly using `bricks` or `ladders`.

While moving from building `i` to building `i+1` (`0`-indexed),

- If the current building's height is **greater than or equal to** the next building's height, you do **not** need a ladder or bricks.

- If the current building's height is **less than** the next building's height, you can either use **one ladder** or `(h[i+1] - h[i])` **bricks**.

*Return the furthest building index (`0`-indexed) you can reach if you use the given ladders and bricks optimally.*


## Classic Solution

It's optimal to always use ladders for those largest jump.

Suppose we have `k` ladders, we can iterate the `heights` and maintain the `k` largest distances using a min-heap. Once we have more than `k` distances, we will use bricks to cover the smaller ones. So once the bricks are run out, this process will terminate. 

```C++
int furthestBuilding(vector<int>& heights, int bricks, int ladders) {
    priority_queue<int, vector<int>, greater<int>> pq;
    for(int i = 0; i < heights.size() - 1; i++) {
        int dis = heights[i+1] - heights[i];
        if(dis > 0) {
            pq.push(dis);
            if(pq.size() > ladders) {
                bricks -= pq.top();
                pq.pop();
                if(bricks < 0)
                    return i;
            }
        }
    }
    return heights.size() - 1;
}
```

