## BFS

### Directed Graphs
- `V` = set of vertices or nodes
- `E` = edges
- `n` = number of vertices, `|V|`
- `m` = number of edges, `|E|`

### Adjacency Matrix representation
- `M = [n][n]` of booleans
    - $M[i][j] = true$ only if `{i, j}` has an edge
- $\mathcal{O}(n^2)$ space
- Better suited for many edges, `m` $\approx \Theta(n^2)$
- Can check `{i, j}` $\in E$ in $\mathcal{O}(1)$ time

### Adjacency List representation
- **The one Kempa uses**
- For each node `i`, we store a list `Adj(i)` containing all neighbors of `i`
- $\mathcal{O}(m)$ space (based on number of edges)
- Better suited for fewer edges, `m` $\approx \Theta(n)$
- Checking `{i, j}` $\in E$ depends on the number of neighbors at node `i`
    - $\mathcal{O}(deg(i))$

### Breadth First Search
- **Input**: Undirected graph $G = (V, E)$ and starting node $s \in V$
- **Output**: Array $d$ containing the length of the shortest path from $s$ to each node $u \in V$

```js
// G = graph
// s = index of starting node in G
function BFS(G, s):

    let d = [n]         // n = number of nodes

    for(let i = 0; i < n; i++):
        d[i] = Infinity

    d[s] = 0

    let Q = new Queue()     // create empty queue

    Q.enqueue(s)

    while !Q.empty:
        let u = Q.dequeue()

        // iterate adjacent nodes to u
        for v in Adj(u):

            // if node has not been visited before
            if d[v] == Infinity:
                d[v] = d[u] + 1
                Q.enqueue(v)
```

### Breadth First Search with tracking
- Adds a new array `prev[n]` that contains the previous node on the path from `s` to any node.
- Can trace the path from any node to `s`.
```js
function BFSWithTracking(G, s):

    let d = [n]
    let prev = [n]          // new

    for(let i = 0; i < n; i++):
        d[i] = Infinity

    d[s] = 0
    prev[s] = null          // starting node has no previous

    let Q = new Queue()

    Q.enqueue(s)

    while !Q.empty:
        let u = Q.dequeue()

        for v in Adj(u):
            if d[v] == Infinity:
                d[v] = d[u] + 1
                prev[v] = u         // new
                Q.enqueue(v)
```

### Two Coloring
- **Input**: undirected graph $G = (V, E)$
- **Output**: boolean, whether or not the vertices in $G$ can be colored using two colors such that two nodes on the same edge do not have the same color.
```js
// attempts to mark each node with 0 or 1
function TwoColoring(G):

    // array c contains the color of each node
    let c = [n]

    let Q = new Queue()

    // initialize each node to have no color
    for(let i = 0; i < n; i++):
        c[i] = -1

    for(let i = 0; i < n; i++):
        if c[i] == -1:
            c[i] = 0
            Q.enqueue(i)

            while !Q.empty():
                let u = Q.dequeue()
                for v in Adj(u):
                    // if v has no color, give it the other color
                    if c[v] == -1:
                        c[v] = 1 - c[u]
                        Q.enqueue(v)

                    // if v has the same color as u, return false
                    else if c[v] == c[u]:
                        return false
    return true
```