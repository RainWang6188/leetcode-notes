# 339. Nested List Weight Sum
## Description

Given a nested list of integers, return the sum of all integers in the list weighted by their depth.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

**Example 1:**
```
Input: [[1,1],2,[1,1]]
Output: 10 
Explanation: Four 1's at depth 2, one 2 at depth 1.
```

**Example 2:**
```
Input: [1,[4,[6]]]
Output: 27 
Explanation: One 1 at depth 1, one 4 at depth 2, and one 6 at depth 3; 1 + 4*2 + 6*3
```

Here's the interface of `NestedInteger` class:
```C++
 // This is the interface that allows for creating nested lists.
 // You should not implement it, or speculate about its implementation
 class NestedInteger {
   public:
     // Return true if this NestedInteger holds a single integer,
     // rather than a nested list.
     bool isInteger() const;

     // Return the single integer that this NestedInteger holds,
     // if it holds a single integer
     // The result is undefined if this NestedInteger holds a nested list
     int getInteger() const;

     // Return the nested list that this NestedInteger holds,
     // if it holds a nested list
     // The result is undefined if this NestedInteger holds a single integer
     const vector<NestedInteger> &getList() const;
 };
```

## My Solution
### 1. DFS

```C++
class Solution {
public:
    int depthSum(const vector<NestedInteger>& nestedList) {
        int res = 0;
        for(int i = 0; i < nestedList.size(); i++) {
            res += dfs(nestedList[i], 1);
        }
        return res;
    }

private:
    int dfs(const NestedInteger& curr_ni, int depth) {
        int curr_res = 0;
        if(curr_ni.isInteger()) {
            curr_res = curr_ni.getInteger() * depth;
        }
        else {
            vector<NestedInteger> curr_list = curr_ni.getList();
            for(int i = 0; i < curr_list.size(); i++) {
                curr_res += dfs(curr_list[i], depth + 1);
            }
        }
        return curr_res;
    }
};
```

### 2. BFS
Here's the BFS version.
```C++
class Solution {
public:
    int depthSum(const vector<NestedInteger>& nestedList) {
        int res = 0;
        for(int i = 0; i < nestedList.size(); i++) {
            res += bfs(nestedList[i]);
        }
        return res;
    }

private:
    int bfs(const NestedInteger& nestedInteger) {
        int res = 0;
        queue<pair<NestedInteger, int>> q;
        q.push(make_pair(nestedInteger, 1));

        while(!q.empty()) {
            const NestedInteger curr_ni = q.front().first;
            int curr_depth = q.front().second;
            q.pop();
            
            if(curr_ni.isInteger()) {
                res += curr_depth * curr_ni.getInteger();
                continue;
            }
            else {
                vector<NestedInteger> curr_list = curr_ni.getList();
                for(int i = 0; i < curr_list.size(); i++) {
                    q.push(make_pair(curr_list[i], curr_depth + 1));
                }
            }
        }
        return res;
    }
```