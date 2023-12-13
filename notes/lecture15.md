## Shortest Paths

- **Input**: Undirected graph $G$ and starting vertex $s$.
- **Output**: Array $d$ such that for any $u \in V$, $d[u]$ = length of the shortest path from $s$ to $u$.
- **Solution**: Breadth First Search

### Dijkstra's Algorithm
- **Input**: Undirected _weighted_ graph $G$ and starting vertex $s$.
    - Assume that the weight of each edge $\geq 0$, no negative weights
- **Output**: Array $d$ such that for any $u \in V$, $d[u]$ = length of the shortest path from $s$ to $u$.
- **Solution**: Dijkstra's Algorithm

```js
function Dijkstra(G, s):

    let d = [V.length]
    
    // initialize shortest paths to max
    for(u in V):
        d[u] = Infinity

    d[s] = 0

    let PQ = new PriorityQueue()

    // add all nodes to PQ
    for(u in V):
        PQ.Insert(u)

    while !PQ.Empty():
        // min: sorted based on d[]
        let u = PQ.ExtractMin()
        for(v in Adj(u)):
            if(d[u] + weight(u, v) < d[v]):

                // distance of next node =
                // distance of current node + weight of edge
                d[v] = d[u] + w(u, v)

                // value in d[] has changed, restructure PQ
                // if binary heap, use HeapUp (log |V|)
                PQ.DecreaseKey(v, d[v])
```
- Time Complexity
    - $|V|$ calls for `ExtractMin`
    - $|E|$ calls (max) for `DecreaseKey`
    - Assuming `PQ` is implemented as a binary heap:
        - $\mathcal{O}((|V| + |E|)\log |V|)$

### Bellman-Ford Algorithm
- **Input**: Undirected _weighted_ graph $G$ and starting vertex $s$.
    - Can contain edges with negative weights, but no negative cycles.
- **Output**: Array $d$ such that for any $u \in V$, $d[u]$ = length of the shortest path from $s$ to $u$.
- **Solution**: Bellman-Ford Algorithm

```js
function BellmanFord(G, s):

    let d = [V.length]
    
    // initialize shortest paths to max
    for(u in V):
        d[u] = Infinity

    d[s] = 0

    for(let k = 0; k < V.length - 1; k++):
        // iterate all edges
        for([u, v] in E):
            if(d[v] > d[u] + weight(u, v)):
                d[v] = d[u] + weight(u, v)
```
- $\mathcal{O}(|V| * |E|)$ time, slower than Dijkstra's algorithm.