## Lecture 4: Suffix Array

### Suffix Array
- An array containing all suffixes of a string $T$, sorted in alphabetical (lexigraphical) order.
- Each index contains the position at which the substring begins, which inherently corresponds to that substring. The suffix array only contains numbers.
```
T = banana

i       SA[i]       T[SA[i]..n]
1       7           $
2       6           a$
3       4           ana$
4       2           anana$
5       1           banana$
6       5           na$
7       3           nana$
```
- If the suffix tree is drawn such that each branch is in lexigraphical order from left to right, a traversal of the tree will give the suffix array.
- Suffix array is not as powerful as suffix tree, only contains a subset of information.
- Anything you can do with a suffix tree, you can do with a suffix array with less space but also less efficiency.

### Suffix Array Algorithms
- $\mathcal{O}(n)$ space.
- $Occ(T, P) \neq 0$
    - binary search in suffix array, $\mathcal{O}(\log n)$
    - string comparison with prefix, $\mathcal{O}(m)$
    - time: $\mathcal{O}(m \log n)$
- $|Occ(T, P)|$
    - binary search for lowest index containing prefix; $b$ = index - 1
    - binary search for highest index $e$ containing prefix
    - return $e - b$
    - time: $\mathcal{O}(m \log n)$
- Enumerate all $i \in Occ(T, P)$
    - find $b$ and $e$ as before
    - iterate through all $i \in (b..e]$
    - time: $\mathcal{O}(m \log n + occ)$
- $\min Occ(T,P)$
    - assume a precomputed RMQ for $SA[1..n]$.
    - find $b$ and $e$.
    - find $j = RMQ(b, e)$
    - return $SA[j]$
    - total query time: $\mathcal{O}(m \log n + t_{RMQ})$

### Range Minimum Query (RMQ)
- ( Treated as a blackbox in lecture )
- Input: array $A[1..m]$ of integers
- $RMQ(b, e)$ returns index $k$ such that $A[k]$ is the minimum value in range $A(b..e]$.
- An RMQ data structure can be constructed.
    - $\mathcal{O}(m)$ time to construct
    - $\mathcal{O}(1)$ query possible
    - $\mathcal{O}(m)$ space

### LCP Array
- $LCP(X, Y)$ = length of longest common prefix of $X$ and $Y$.
- $LCP[1] = 0$
- $LCP[i]$ = LCP of string at $SA[i]$ and $SA[i-1]$
```
T = banana

i       SA[i]       LCP[i]      T[SA[i]..n]
1       7           0           $
2       6           0           a$
3       4           1           ana$
4       2           3           anana$
5       1           0           banana$
6       5           0           na$
7       3           2           nana$
```

### Number of Distinct Substrings
- $|Substr(T)| = \frac{n(n+1)}{2}$ − all values in LCP array, where $n$ = number of characters in $T$
```
T = banana

b       a       n       a'      n'      a'      // remove {a, n, a}
ba      an      na      an'     na'             // remove {an, na}
ban     ana     nan     ana'                    // remove {ana}
bana    anan    nana
banan   anana
banana

n(n+1)/2
= 6(7)/2
= 21

21 - 3 - 2 - 1
= 15 distinct substrings
```