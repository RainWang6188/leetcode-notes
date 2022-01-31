# 207. Course Schedule

## Description
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array prerequisites where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.

For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.
Return `true` if you can finish all courses. Otherwise, return `false`.


**Example:**
```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: true
Explanation: There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
```

## My Solution
### 1. Topological Sort
We can solve it by sorting the courses in a topological order and check if the count of the result valid courses is equal to `numCourses`.

In the implementation of topological sort, I use a vector to store the in degree of all the courses and update them during the queue operation by traversing the `prerequisites` array. So it's not time-efficient...

```C++
bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
    vector<int> in_degree(numCourses, 0);
    int count = 0;
    
    for(auto& req : prerequisites)
        in_degree[req[1]]++;
    
    queue<int> q;
    for(int i = 0; i < numCourses; i++) {
        if(!in_degree[i]) {
            q.push(i);
            count++;
        }
    }
    
    while(!q.empty()) {
        int cur = q.front();
        q.pop();
        
        for(auto& req: prerequisites) {
            if(req[0] == cur && --in_degree[req[1]] == 0) {
                q.push(req[1]);
                count++;
            }
        }
    }
    return count == numCourses;
}
```

### 2. DFS for a cycle


## Classic Solution
Here's a better way to implement topological sort.

We record the map in an adjacent matrix `adj[][]`, where `adj[i]` stores the course that must be taken after `i`.

As a result, in the queue operation, we simply need to decrement of in degree of the courses in `adj[cur]`, instead of searching for all the courses that should be updated

```C++
bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
    int count = 0;
    vector<vector<int>> adj(numCourses, vector<int>());
    vector<int> in_degree(numCourses, 0);
    
    for(auto& req : prerequisites) {
        adj[req[1]].push_back(req[0]);
        in_degree[req[0]]++;
    }
    
    queue<int> q;
    for(int i = 0; i < numCourses; i++)
        if(!in_degree[i])
            q.push(i);
    
    while(!q.empty()) {
        int curr = q.front();
        q.pop();
        count++;
        
        for(auto& next : adj[curr])
            if(--in_degree[next] == 0)
                q.push(next);
    }
    return count == numCourses;
}
```