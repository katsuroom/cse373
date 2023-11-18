## DFS

### Depth First Search
- Follow one path to the end before backtracking to visit the remaining nodes.
- $\mathcal{O}(|V| + |E|)$ runtime

```js
// calculates distance from start node

let color = [n]         // color of node
let prev = [n]          // previous node from i to start
let d = [n]             // distance from start node to i
let f = [n]             // finishing time for node i 

let time = undefined

function DFS(G, s):

    for(let i = 0; i < n; i++):
        color[i] = WHITE        // WHITE = not visited
        prev[i] = null

    time = 1

    for(let i = 0; i < n; i++):
        if color[i] == WHITE :
            DFSVisit(G, i)



function DFSVisit(G, u):
    d[u] = time++
    color[u] = GRAY             // GRAY = visited, not finished processing

    for v in Adj(u):
        if color[v] == WHITE :
            prev[v] = u
            DFSVisit(G, v)

    color[u] = BLACK            // BLACK = visited and fully processed
    f[u] = time++
```

### Directed Acyclic Graph
- A directed graph without cycles.

### Topological Sort
- **Input**: A directed acyclic graph $G = (V, E)$
- **Output**: A node sequence such that if $(u, v) \in E$, then $u$ occurs before $v$ in the sequence
![topological_sort](../img/topological_sort.png)
- Lemma
    - For every edge $(u, v) \in E$ in a DAG, it holds that $f[u] > f[v]$
- Solution
    - Start with an empty list $L$.
    - Run DFS to compute $f[u]$ for every node $u \in V$.
    - Each time $f[u]$ is computed, push $u$ at the front of $L$.
    - return $L$