# 1514. Path with Maximum Probability

## Description
You are given an undirected weighted graph of `n` nodes (**0-indexed**), represented by an edge list where `edges[i] = [a, b]` is an undirected edge connecting the nodes `a` and `b` with a probability of success of traversing that edge `succProb[i]`.

Given two nodes `start` and `end`, find the path with the maximum probability of success to go from `start` to `end` and return its success probability.

If there is no path from `start` to `end`, return `0`. Your answer will be accepted if it differs from the correct answer by at most `1e-5`.
## My Solution
This problem asks us to find the path with maximum probability product, so we can use Dijkstra's algorithm and find the maximum product instead of minimum sum.

Here's my implementation of Dijkstra's algorithm with max heap optimization.


```C++
typedef pair<int, double> edge_pair;
typedef pair<double, int> queue_pair;

class Solution {
public:
    double maxProbability(int n, vector<vector<int>>& edges, vector<double>& succProb, int start, int end) {
        vector<double> prob(n, 0);
        
        dijkstra(n, edges, succProb, start, end, prob);
        
        return prob[end];
    }

private:
    priority_queue<queue_pair, vector<queue_pair>, less<queue_pair>> pq;
    
    void dijkstra(int n, vector<vector<int>>& edges, vector<double>& succProb, int start, int end, vector<double>& prob) {
        vector<vector<edge_pair>> adj(n);
        build_graph(adj, edges, succProb);
        
        prob[start] = 1;
        pq.push(make_pair(1, start));
        
        while(!pq.empty()) {
            int pos = pq.top().second;
            pq.pop();
            
            for(auto& next_pair : adj[pos]) {
                int next_pos = next_pair.first;
                double next_p = next_pair.second;
                
                if(prob[next_pos] < prob[pos] * next_p) {
                    prob[next_pos] = prob[pos] * next_p;
                    pq.push(make_pair(prob[next_pos], next_pos));
                }
            }
        }
        
    }
    
    void build_graph(vector<vector<edge_pair>>& adj, vector<vector<int>>& edges, vector<double>& succProb) {
        for(int i = 0; i < edges.size(); i++) {
            int v1 = edges[i][0];
            int v2 = edges[i][1];
            double p = succProb[i];
            
            adj[v1].push_back(make_pair(v2, p));
            adj[v2].push_back(make_pair(v1, p));
        }
    }
};
```