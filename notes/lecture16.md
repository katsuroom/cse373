## All Pairs Shortest Paths

### Problem
- **Input**: Undirected _weighted_ graph $G$.
- **Output**: 2D array $d$ such that for any pair $u, v \in V$, $d[u][v]$ = length of the shortest path from $u$ to $v$.
- **Solutions**
    - Definitions
        - APSP: All Pairs Shortest Paths
        - SSSP: Single Source Shortest Paths
    - If weights are nonnegative, we can run Dijkstra's algorithm $|V|$ times.
        - Total time: $\mathcal{O}(|V||E|\log |V|)$
    - If contains negative weights, we can run the Bellman-Ford algorithm $|V|$ times.
        - Total time: $\mathcal{O}(|V|^2 * |E|)$, up to $\mathcal{O}(|V|^4)$ for dense graphs.
    - Better solution: Floyd-Warshall Algorithm
        - Running time: $\mathcal{O}(|V|^3)$

### Floyd-Warshall Algorithm
- Dynamic Programming
- For each $(i, j) \in V$, let $w_{i, j}$ =
```py
if i == j:                          w = 0
if i != j and {i, j} has edge:      w = weight(i, j)
else                                w = Infinity
```
```js
function FloydWarshall(G):

    let n = V.length

    // for containing weight for every i, j
    w = [n][n]

    // memo array
    D = [n][n][n]

    for(let i = 0; i < n; i++):
        for(let j = 0; j < n; j++):
            calculateWeight(i, j)
            D[0][i][j] = w[i][j]

    for(let k = 0; k < n; k++):
        for(let i = 0; i < n; i++):
            for(let j = 0; j < n; j++):
                D[k][i][j] = min(
                    D[k-1][i][k] + D[k-1][k][j],
                    D[k-1][i][j])
    
    // returns 2D array D[k][ ][ ]
    return D[k]
```
- Time: $\mathcal{O}(n^3)$
- Space: $\mathcal{O}(n^3)$, can be reduced to $\mathcal{O}(n^2)$ if we only store `D[k][][]` and `D[k-1][][]`