# 133. Clone Graph

## Description
Given a reference of a node in a **connected** undirected graph.

Return a **deep copy** (clone) of the graph.

Each node in the graph contains a value (int) and a list (List[Node]) of its neighbors.
```java
class Node {
    public int val;
    public List<Node> neighbors;
}
```

**Test case format:**

For simplicity, each node's value is the same as the node's index (1-indexed). For example, the first node with `val == 1`, the second node with `val == 2`, and so on. The graph is represented in the test case using an adjacency list.

**An adjacency list** is a collection of unordered **lists** used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with `val = 1`. You must return the copy of the given node as a reference to the cloned graph.


**Example:**
```
Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
Explanation: There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
```
## My Solution
My first thought is doing a breadth-first search fron the start vertex. Each time when we pop a node `cur` from the queue, we new a copy of its neighbors and push it into the queue. But two questions occurs:

- During the search, current node `cur` may have a previously traversed node `prev` as its neighbor, how to find the newly generated copy of `prev`?

- When to terminate the loop and how?

My solution to the first question is to use hashing. Once we new a copy `clone_n` of a node `n`, we preserve the relationship in a hashmap by setting `map[n] = clone_n`. Then finding a previously newed node is with ease.

By using the hashmap, the second quesion is solved easily. Since the loop need to end if every node has been pushed into the queue once, we can only push the neighbors which doesn't has a hash relation.

```C++
Node* cloneGraph(Node* node) {
    if(!node)
        return nullptr;
    
    map<Node*, Node*> dict;
    Node* clone_node = new Node(node->val);
    dict[node] = clone_node;
    
    queue<Node*> q;
    q.push(node);
    
    while(!q.empty()) {
        Node* cur = q.front();
        q.pop();
        
        for(auto neighbor: cur->neighbors) {
            if(dict.find(neighbor) == dict.end()) {
                Node* clone_neighbor = new Node(neighbor->val);
                dict[neighbor] = clone_neighbor;
                q.push(neighbor);
            }
            (dict[cur]->neighbors).push_back(dict[neighbor]);
        }
    }
    return clone_node;
}
```

## Classic Solution
We can also traverse the graph by depth-first search.

The following succinct DFS code is taken from [this post](https://leetcode.com/problems/clone-graph/discuss/42362/9-line-c%2B%2B-DFS-Solution).

```C++
class Solution {
public:
    Node* cloneGraph(Node* node) {
        if (!node) {
            return NULL;
        }
        if (copies.find(node) == copies.end()) {
            copies[node] = new Node(node -> val, {});
            for (Node* neighbor : node -> neighbors) {
                copies[node] -> neighbors.push_back(cloneGraph(neighbor));
            }
        }
        return copies[node];
    }
private:
    unordered_map<Node*, Node*> copies;
};
```