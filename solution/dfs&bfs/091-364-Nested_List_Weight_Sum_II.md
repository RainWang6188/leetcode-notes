# 364. Nested List Weight Sum II
## Description
Given a nested list of integers, return the sum of all integers in the list weighted by their depth.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

Different from the [previous](https://www.lintcode.com/problem/551/) question where weight is increasing from root to leaf, now the weight is defined from bottom up. i.e., the leaf level integers have weight 1, and the root level integers have the largest weight.

**Example 1:**
```
Input: 
[[1,1],2,[1,1]]
Output: 8 

Explanation: 
Four 1's at depth 1, one 2 at depth 2.
```

**Example 2:**
```
Input: 
[1,[4,[6]]]
Output: 17 

Explanation:
One 1 at depth 3, one 4 at depth 2, and one 6 at depth 1; 1*3 + 4*2 + 6*1 = 17.
```

## My Solution
### 1. DFS
We store the integer by layers during the DFS and then reverse the layers so that we can add up to the final result easily.
```C++
class Solution {
public:
    /**
     * @param nestedList: a list of NestedInteger
     * @return: the sum
     */
    int depthSumInverse(vector<NestedInteger> nestedList) {
        int res = 0;
        vector<vector<int>> layers;
        dfs(nestedList, 1, layers);
        reverse(layers.begin(), layers.end());
        for(int i = 0; i < layers.size(); i++) {
            for(int val : layers[i])
                res += (i + 1) * val;
        }
        return res;
    }

private:
    void dfs(vector<NestedInteger> &nestedList, int depth, vector<vector<int>> &layers) {
        if(layers.size() < depth)
            layers.push_back({});
        
        for(auto node : nestedList) {
            if(node.isInteger()) {
                layers[depth-1].push_back(node.getInteger());
            }
            else {
                vector<NestedInteger> next_list = node.getList();
                dfs(next_list, depth+1, layers);
            }
        }
    }
};
```

### 2. BFS
The BFS version is more convenient to implement.

We traverse the vector in level-order and use `level_sum` to store the total sum on the current level and add it up to the `prev_sum`. Since the weight is decreasing, in each level we add the previous sum (`prev_level`) to the result to simulate the weight decrease.

```C++
int depthSumInverse(vector<NestedInteger> nestedList) {
    int res = 0;
    int prev_sum = 0;
    queue<NestedInteger> q;
    for(auto ni : nestedList)
        q.push(ni);
    
    while(!q.empty()) {
        int level_sum = 0;
        int level_size = q.size();
        for(int i = 0; i < level_size; i++) {
            NestedInteger curr = q.front();
            q.pop();
            if(curr.isInteger())
                level_sum += curr.getInteger();
            else {
                for(auto next_ni : curr.getList())
                    q.push(next_ni);
            }
        }
        prev_sum += level_sum;
        res += prev_sum;
    }
    return res;
}
```
