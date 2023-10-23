## Lecture 7: Edit Distance

### Dynamic Programming
- Optimizing recursive functions so that previously computed values are stored and do not require recomputing.
- Naive Fibonacci
    - Time: $\Theta(\varphi^n)$, where $\varphi = 1.61$
    - Space: $\mathcal{O}(n)$
```py
def Fib(n):
    if n <= 1:
        return n
    else:
        return Fib(n - 1) + Fib(n - 2)
```
- Dynamic programming via memoizing
    - Create an array for storage.
    - Time: $\mathcal{O}(n)$
    - Space: $\mathcal{O}(n)$
```py
# premake an array of size m
memo = []

for i in range(0, m):
    memo[i] = -1

def Fib(n):                 # n <= m
    if memo[n] != -1:
        return memo[n]

    ret = -1
    if n <= 1:
        ret = n
    else:
        ret = Fib(n - 1) + Fib(n - 2)

    memo[n] = ret
    return ret

```
- Compute values in order from bottom-up
    - Store only the values that are needed.
    - Time: $\mathcal{O}(n)$
    - Space: $\mathcal{O}(1)$
```py
def Fib(n):

    if n <= 1:
        return n

    prev = 0
    curr = 1

    for i in range(2, n+1):
        new = prev + curr
        prev = curr
        curr = new

    return curr
```

### Edit Distance
- `ED(X, Y)` = smallest number of insertions, deletions, and substitutions in $X$ to obtain $Y$.
```
Ex.
X = GTTACTCGA
Y = GCTTGCCG

G - T T A C T C G A
|   | |   |   | |
G C T T G C - C G -

ED(X, Y) = 4
```
- Lemma
    - For any string $X$, the edit distance between $X$ and the empty string $\epsilon$ is the length of $X$.
        - $\forall x.\quad ED(X, \epsilon) = ED(\epsilon, X) = |X|$
    - For all nonempty $X \in \Sigma^m$ and $Y \in \Sigma^n$:
        - `ED(X, Y) = min(a, b, c)`
        - `a = ED(X[1...m-1], Y[1...n-1]) + (X[m] == Y[n] ? 1 : 0)`
        - `b = ED(X[1...m-1], Y[1...n]) + 1`
        - `c = ED(X[1...m], Y[1...n-1]) + 1`
```py
# X = string of length m
# Y = string of length n

# suddenly arrays are zero-indexed, bruh Kempa
def EditDistance(X, m, Y, n):

    for i in range(0, m+1):             # [0, m]
        A[i, 0] = i

    for j in range(1, n+1):             # [1, n]
        A[0, j] = j

    for i in range(1, m+1):             # [1, m]
        for j in range(1, n+1):         # [1, n]
            A[i, j] = A[i-1, j-1] + (X[i] == Y[i] ? 1 : 0)
            A[i, j] = min(A[i, j], A[i-1, j] + 1)
            A[i, j] = min(A[i, j], A[i, j-1] + 1)

    return A[m, n]
```
- $\mathcal{O}(mn)$ time
- $\mathcal{O}(mn)$ space

```
Ex.
X = ABCDEF          m = 6
Y = AZCED           n = 5

concept:
A   B   C   D   E   F
    v       x       v           v = change, x = delete
A   Z   C       E   D

ED(X, Y) should be 3.

A[][] initial setup:

        A   B   C   D   E   F
    0   1   2   3   4   5   6
A   1   -   -   -   -   -   -
Z   2   -   -   -   -   -   -
C   3   -   -   -   -   -   -
E   4   -   -   -   -   -   -
D   5   -   -   -   -   -   -


First iteration:
X = A
Y = A
Requires 0 edits

        A   B   C   D   E   F
    0   1   2   3   4   5   6
A   1   0   -   -   -   -   -
Z   2   -   -   -   -   -   -
C   3   -   -   -   -   -   -
E   4   -   -   -   -   -   -
D   5   -   -   -   -   -   -


Second iteration:
X = AB
Y = A
Requires 1 edit, delete B.

        A   B   C   D   E   F
    0   1   2   3   4   5   6
A   1   0   1   -   -   -   -
Z   2   -   -   -   -   -   -
C   3   -   -   -   -   -   -
E   4   -   -   -   -   -   -
D   5   -   -   -   -   -   -


Further iterations in the same row:
X = ABCDEF
Y = A
Requires m - 1 deletions.

        A   B   C   D   E   F
    0   1   2   3   4   5   6
A   1   0   1   2   3   4   5
Z   2   -   -   -   -   -   -
C   3   -   -   -   -   -   -
E   4   -   -   -   -   -   -
D   5   -   -   -   -   -   -


First iteration in second row:
X = A
Y = AB

        A   B   C   D   E   F
    0   1   2   3   4   5   6
A   1   0   1   2   3   4   5
Z   2   ?   -   -   -   -   -
C   3   -   -   -   -   -   -
E   4   -   -   -   -   -   -
D   5   -   -   -   -   -   -

Rule:
For any A[i][j], check the value to the left, up, and upper left.
If the current character at X and Y are the same, A[i][j] = the minimum of the three values.
Otherwise, A[i][j] = the minimum of the three values + 1.

A != Z, minimum of the three surrounding values is 0, then add 1.

        A   B   C   D   E   F
    0   1   2   3   4   5   6
A   1   0   1   2   3   4   5
Z   2   1   -   -   -   -   -
C   3   -   -   -   -   -   -
E   4   -   -   -   -   -   -
D   5   -   -   -   -   -   -


Last iteration:

        A   B   C   D   E   F
    0   1   2   3   4   5   6
A   1   0   1   2   3   4   5
Z   2   1   1   2   3   4   5
C   3   2   2   1   2   3   4
E   4   3   3   2   2   2   3
D   5   4   4   3   2   3   3

ED(X, Y) = bottom-right most value = 3
```

### Retrieving Alignment for Edit Distance
- Start from $A[m, n]$ and traverse to $A[0, 0]$, moving only left, up, or upper left in the direction of the minimum value.
    - `L`, `D`, `D'`, `U`
    - `D` if the value remains the same, `D'` if the value changes.
    - Store the string during the traversal, then reverse.
    - Meaning of directions:
        - `L`: Add new character in X
        - `U`: Add new character in Y
        - `D`: Keep the same character, no change
        - `D'`: Change the character from X to Y
- Iterate through string:
    - `L`: Character above, `-` below.
    - `D`: Same character above and below.
    - `D'`: Character above, different character below.
    - `U`: `-` above, character below.
- Alignment:
    `|` for `D`

```
Ex.
X = ABCDEF
Y = AZCED

        A   B   C   D   E   F
    0   1   2   3   4   5   6
A   1   0   1   2   3   4   5
Z   2   1   1   2   3   4   5
C   3   2   2   1   2   3   4
E   4   3   3   2   2   2   3
D   5   4   4   3   2   3   3

        A   B   C   D   E   F
    0*  1   2   3   4   5   6
A   1   0*  1   2   3   4   5
Z   2   1   1*  2   3   4   5
C   3   2   2   1*  2*  3   4
E   4   3   3   2   2   2*  3
D   5   4   4   3   2   3   3*

3   -> 2    -> 2    -> 1    -> 1    -> 0    -> 0
    D'      D       L       D       D'      D

Reverse:
D   D'  D   L   D   D'

Alignment:
A   B   C   D   E   F
|       |       |
A   Z   C   -   E   D
```

- If only the last two rows/columns are kept, space can be reduced to $\mathcal{O}(n)$.
    - It can no longer be used to obtain the alignment.
```py
def EditDistance(X, m, Y, n):
    b = 0
    for j in range (0, n+1):            # [0, n]
        A[b, j] = j
    for i in range(1, m+1):             # [1, m]
        b = 1 - b                       # switches between 1 and 0
        A[b, 0] = i
        for j in range(1, n):           # [1, n]
            A[b, j] = A[1-b, j-1] + (X[i] == Y[j] ? 1 : 0)
            A[b, j] = min(A[b, j], A[1-b, j] + 1)
            A[b, j] = min(A[b, j], A[b, j-1] + 1)

    return A[b,n]
```