# 0. Union Find
- Definition: Given a set $S$ and some relations, find all the equivalence classes.

- Algorithms: `S[v] = v's parent` and `S[root] = -1`
    
    - Set Union
        ```C++
        void SetUnion(DisSet S, SetType Rt1, SetType Rt2) {
            S[Rt1] = Rt2;
        }
        ```
    - Find
        ```C++
        ElementType Find(DisSet S, ElementType X) {
            while(S[X] != -1)
                X = S[X];
            return X;
        }
        ```
- Improvement

    - smart union algorithms

        - Union by Size
            `S[root] = -size`, always change the smaller tree

        - Union by Height
            `S[root] = -height`, always change the shallow tree

    - Path Compression

        Direct each element `X` to the `root` during the `Find()` operation.

        Slow for singal find, but faster for a sequence of find operations.

        - Recursive Version
        ```C++
        ElementType Find(DisSet S, ElementType X) {
            if(S[X] < 0)
                return X;
            else
                X = Find(S, S[X]);
        }
        ```

        - Iterative Version
        ```C++
        ElementType Find(DisSet S, ElementType X) {
            ElementType root, tail, lead;
            for(root = X; S[root] >= 0 root = S[root])
                ;
            for(tail = X; tail != root; tail = lead) {
                lead = S[tail];
                S[tail] = root;
            }
            return root;
        }
        ```


# 1. Topological Sort
- Definition: A **topological order** is a linear ordering of the vertices of a graph such that, for any two vertices, $i, j$, if $i$ is a predecessor of $j$ in the network, then $i$ precedes $j$ in the linear ordering.

- Algorithm

    Keep all the unassigned vertices of degree $0$ in a container (queue or stack).

    ```C++
    void TopSort(Graph G) {
        int counter = 0;
        Q = CreateQueue(NumVertex); MakeEmpty(Q);
        for(each vertex V)
            if(inDegree[V] == 0)    Enqueue(V, Q);
        while(!isEmpty(Q)) {
            V = Dequeue(Q);
            TopNumber[V] = ++counter;
            for(each W adjacent to V)
                if(--inDegree[W] == 0)  Enqueue(W, Q);
        }
        if(counter != NumVertex)
            Error("Graph has a cycle!");
        free(Queue);
    }
    ```
- Time Complexity: $O(|E|+|V|)$
# 2. Shortest Path Algorithm
> Assume no negative cost for simplicity.
> 
> Some helpful posts can be viewed [here](https://zhuanlan.zhihu.com/p/33162490)
## 2.1 Single Source Shortest-Path Problem

### 2.1.1 Unweighted Shortest Path
- Algorithm: BFS, always update the adjacent vertices at each iteration.
```C++
void Unweighted(Table T) {
    Q = CreateQueue(NumVertex); MakeEmpty(Q);

    /*Enqueue the start vertex S, determined elsewhere*/
    Enqueue(S, Q);

    while(!isEmpty(Q)) {
        Vertex V = Dequeue(Q);

        for(each vertex W adjacent to V) {
            if(T[W].Dist == Infinity) {
                T[W].Dist = T[V].Dist + 1;
                T[W].Path = V;
                Enqueue(W, Q);
            }
        }
    }
    free(Q);
}
```
### 2.1.2 Weighted Shortest Path
#### 2.1.2.1 **Dijkstra's Algorithm**
    
- Algorithm([wiki](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm))

    Only suits for **non-negative** paths. Dijkstra's shortest path algorithm is a **greedy** algorithm. The procedure of the algorithm is as follows:
        
    1. Use an array `dis[]` to record the shortest distance from `s` to any other vertices. We initialize every distance as `inf` except `dis[s]=0`.
    2. We select the **unvisited** vertex `u` with the **smallest distance**, mark it as visited.
    3. Traverse each adjacent vertices `v` of `u`, update the distance `dis[v]` if `dis[v] > dis[u] + len(u, v)`.
    4. Repeat step 2 and 3 until every vertex is visited.

- Pseudocode
    ```C++
    void Dijkstra(Table T) {
        while(1) {
            V = smallest unknown distance vertex;
            if(V == NotAVertex)
                break;
            T[V].Known = true;
            for(each W adjacent to V)
                if(!T[W].Known)
                    if(T[V].Dist + C(V, W) < T[W].Dist) {
                        T[W].Dist = T[V].Dist + C(V, W);
                        T[W].Path = V;
                    }
        }
    }
    ```

- Implementation to find the smallest unknown distance vertex

    - Implementation 1 - Simply scan the table
        
        It takes $O(|V|)$ to find the smallest vertex, the whole algorithm takes $O(|E| + |V|^2)$, good if the graph is dense.
    - Implementation 2 - Keep distance in a **Priority Queue**

        It takes $O(\log |V|)$ to pop the smallest vertex from a min heap, the whole algorithm takes $O((|E| + |V|)\log |V|)$, good if the graph is sparse.

        However, update the priority of a specific element can be very tricky... To resolve that issue, we simply ignore the previous distance `dis[i]` in the priority queue while inserting the shorter distance of `dis[i]`. So we allow multiple instances of same vertex in priority queue. This approach doesn’t require decrease key operation and has below important properties.

        - Whenever distance of a vertex is reduced, we add one more instance of vertex in priority_queue. Even if there are multiple instances, we only consider the instance with minimum distance and ignore other instances.
        - The time complexity remains $O(|E|\log |V|))$ as there will be at most $O(|E|)$ vertices in priority queue and $O(\log |E|)$ is same as $O(\log |V|)$.
        
         [Here](https://www.geeksforgeeks.org/dijkstras-shortest-path-algorithm-using-priority_queue-stl/) is an implementation for reference.

#### 2.1.2.2 **Bellman-Ford Algorithm**
- Algorithm([wiki](https://en.wikipedia.org/wiki/Bellman%E2%80%93Ford_algorithm))

    This algorithm can be used in **negative-weighted** graph or even **negative-cyclic** graph. It employs the dynamic progamming thoughts.

    Like Dijkstra's algorithm, Bellman–Ford proceeds by **relaxation**, in which approximations to the correct distance are replaced by better ones until they eventually reach the solution.

    Simply put, the algorithm initializes the distance from the `source` to `0` and all other nodes to `infinity`. 
    
    Then for all edges, if the distance to the destination can be shortened by taking the edge, the distance is updated to the new lower value. At each iteration `i` that the edges are scanned, the algorithm finds all shortest paths of at most length `i` edges (and possibly some paths longer than `i` edges). Since the longest possible path without a cycle can be $|V|-1$ edges, the edges must be scanned $|V|-1$ times to ensure the shortest path has been found for all nodes. 
    
    A final scan of all the edges is performed and if any distance is updated, then a path of length $|V|$ edges has been found which can only occur if at least one negative cycle exists in the graph.

- Pseudocode
    ```C++
    function BellmanFord(list vertices, list edges, vertex source) is

        // This implementation takes in a graph, represented as
        // lists of vertices (represented as integers [0..n-1]) and edges,
        // and fills two arrays (distance and predecessor) holding
        // the shortest path from the source to each vertex

        distance := list of size n
        predecessor := list of size n

        // Step 1: initialize graph
        for each vertex v in vertices do

            distance[v] := inf             // Initialize the distance to all vertices to infinity
            predecessor[v] := null         // And having a null predecessor
        
        distance[source] := 0              // The distance from the source to itself is, of course, zero

        // Step 2: relax edges repeatedly
        
        repeat |V|−1 times:
            for each edge (u, v) with weight w in edges do
                if distance[u] + w < distance[v] then
                    distance[v] := distance[u] + w
                    predecessor[v] := u

        // Step 3: check for negative-weight cycles
        for each edge (u, v) with weight w in edges do
            if distance[u] + w < distance[v] then
                error "Graph contains a negative-weight cycle"

        return distance, predecessor
    ```

- Time Complexity: $O(|E|\cdot |V|)$

#### 2.1.2.3 **SPFA** (Shortest Path Fast Algorithm)

- Algorithm

    The Shortest Path Faster Algorithm (SPFA) is an improvement of the Bellman–Ford algorithm. The improvement over the latter is that instead of trying all vertices blindly, SPFA maintains a queue of candidate vertices and adds a vertex to the queue only if that vertex is relaxed. This process repeats until no more vertex can be relaxed.

- Pesudocode
    Below is the pseudo-code of the algorithm. Here $G=(V,E)$ is a weighted directed graph and a source vertex $s$. The length of the shortest path from $s$ to $v$ is stored in $d(v)$ for each vertex $v$. $Q$ is a first-in, first-out queue of candidate vertices, and $w(u,v)$ is the edge weight of $(u,v)$.

    Note that we can use an array to record if each vertex is in the heap.

    ```C++
    function Shortest-Path-Faster-Algorithm(G, s)
    1    for each vertex v ≠ s in V(G)
    2        d(v) := ∞
    3    d(s) := 0
    4    push s into Q
    5    while Q is not empty do
    6        u := poll Q
    7        for each edge (u, v) in E(G) do
    8            if d(u) + w(u, v) < d(v) then
    9                d(v) := d(u) + w(u, v)
    10                if v is not in Q then
    11                    push v into Q
    ```

- Time Complexity: The average running time is $O(|E|)$, which is indeed true on random graphs. But the worst-case running time is $O(|E|\cdot |V|)$, just like that the standard Bellman-Ford algorithm.

## 2.2 Multi-source Shortest Path Problem
### 2.2.1 Floyd's Algorithm

Floyd's Algorithm (aslo known as Floyd–Warshall algorithm) is an algorithm for finding shortest paths in a directed weighted graph with positive or negative edge weights (but with no negative cycles)

- Algorithm

    Consider a graph $G$ with vertices $V$ numbered $1$ through $N$. Further consider a function $\mathrm {shortestPath} (i,j,k)$ that returns the shortest possible path from $i$ to $j$ using vertices only from the set $\{1,2,\ldots ,k\}$ as intermediate points along the way. Now, given this function, our goal is to find the shortest path from each $i$ to each $j$ using any vertex in $\{1,2,\ldots ,N\}$.

    For each of these pairs of vertices, the $\mathrm {shortestPath} (i,j,k)$ could be either

    (1) a path that does not go through $k$ (only uses vertices in the set $\{1,\ldots ,k-1\}$.)

    or

    (2) a path that does go through $k$ (from $i$ to $k$ and then from $k$ to $j$, both only using intermediate vertices in $\{1,\ldots ,k-1\}$)

    As a result, we can use dynamic programming and here's the state transition function with basic case $\mathrm{shortestPath}(i,j,0) = w(i,k)$.
    $$
        \mathrm{shortestPath}(i, j, k) = 
        \min {\Big (}
                \mathrm{shortestPath}(i, j, k-1),\   
                \mathrm{shortestPath}(i, k, k-1) + \mathrm{shortestPath}(k, j, k-1)
             {\Big )}
    $$

- Pseudocode

``` C++
let dist be a |V| × |V| array of minimum distances initialized to ∞ (infinity)
for each edge (u, v) do
    dist[u][v] ← w(u, v)  // The weight of the edge (u, v)
for each vertex v do
    dist[v][v] ← 0
for k from 1 to |V|
    for i from 1 to |V|
        for j from 1 to |V|
            if dist[i][j] > dist[i][k] + dist[k][j] 
                dist[i][j] ← dist[i][k] + dist[k][j]
            end if
```

# 3.  Connected Components
- Definition: In graph theory, a **connnected component** of an undirected graph is a connected subgraph that is not part of any larger connected subgraph

## 3.1 Connected Components in an Undirected Graph
Finding connected components for an undirected graph is an easier task. We simple need to do either BFS or DFS starting from every unvisited vertex, and we get all strongly connected components. Below are steps based on DFS.
```
1) Initialize all vertices as not visited.
2) Do following for every vertex 'v'.
       (a) If 'v' is not visited before, call DFSUtil(v)
       (b) Print new line character

DFSUtil(v)
1) Mark 'v' as visited.
2) Print 'v'
3) Do following for every adjacent 'u' of 'v'.
     If 'u' is not visited, then recursively call DFSUtil(u)
```

## 3.2 Strongly Connected Components in Directed Graph
- Definition: A directed graph is **strongly connected** if there is a path between all pairs of vertices. A strongly connected component (SCC) of a directed graph is a **maximal** strongly connected subgraph.

### 3.2.1 Kosaraju's Algorithm

We can find all strongly connected components in `O(V+E)` time using Kosaraju’s algorithm. Following is detailed Kosaraju’s algorithm.
1) Create an empty stack `S` and do DFS traversal of a graph. In DFS traversal, **after calling recursive DFS for adjacent vertices of a vertex**, push the vertex to stack. 
2) Reverse directions of all arcs to obtain the transpose graph.
3) One by one pop a vertex from S while S is not empty. Let the popped vertex be `v`. Take v as source and do DFS (call DFSUtil(v)). The DFS starting from v prints strongly connected component of v.

An implementation of Kosaraju's algorithm is [here](https://www.geeksforgeeks.org/strongly-connected-components/).

### 3.2.2 Tarjan's Algorithm
[Here](https://www.geeksforgeeks.org/tarjan-algorithm-find-strongly-connected-components/) is a post about Tarjan's algorithm.