# 261. Graph Valid Tree

## Description
Given `n` nodes labeled from `0` to `n-1` and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

**Example:**
```
n = 5
edges = [[0,1], [0,2], [0,3], [1,4]]
```
Note: you can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0,1] is the same as [1,0] and thus will not appear together in edges.
## Classic Solution

这道题给了一个无向图，让我们来判断其是否为一棵树，如果是树的话，所有的节点必须是连接的，也就是说必须是连通图，而且不能有环，所以焦点就变成了验证是否是连通图和是否含有环。

用 DFS 来做，根据 pair 来建立一个图的结构，用邻接链表来表示，还需要一个一位数组v来记录某个结点是否被访问过，然后用 DFS 来搜索结点0，遍历的思想是，当 DFS 到某个结点，先看当前结点是否被访问过，如果已经被访问过，说明环存在，直接返回 false，如果未被访问过，现在将其状态标记为已访问过，然后到邻接链表里去找跟其相邻的结点继续递归遍历，**注意还需要一个变量 pre 来记录上一个结点，以免回到上一个结点**，这样遍历结束后，就把和结点0相邻的节点都标记为 true，然后再看v里面是否还有没被访问过的结点，如果有，则说明图不是完全连通的，返回 false，反之返回 true，参见代码如下：

```C++
// DFS
class Solution {
public:
    bool validTree(int n, vector<pair<int, int>>& edges) {
        vector<vector<int>> g(n, vector<int>());
        vector<bool> v(n, false);
        for (auto a : edges) {
            g[a.first].push_back(a.second);
            g[a.second].push_back(a.first);
        }
        if (!dfs(g, v, 0, -1)) return false;
        for (auto a : v) {
            if (!a) return false;
        }
        return true;
    }
    
    bool dfs(vector<vector<int>> &g, vector<bool> &v, int cur, int pre) {
        if (v[cur]) return false;
        v[cur] = true;
        for (auto a : g[cur]) {
            if (a != pre) {
                if (!dfs(g, v, a, cur)) return false;
            }
        }
        return true;
    }
};
```