## Lecture 8: Knapsack

### Subset Sum
- Given an array $X$ of $n \geq 0$ positive integers and a nonnegative integer $T$, output `TRUE` if there exists a subset of elements of $X$ that sums to $T$, and `FALSE` otherwise.
```
ex.

X = [4, 2, 7, 4, 9, 2]
T = 20
TRUE (20 = 9 + 7 + 4)

X = [4, 2, 7, 4, 9, 2]
T = 40
FALSE
```
- Lemma for $SSum(X, n, T)$:
    - For every $X$, $SSum(X, n, 0) = $ `TRUE`
    - For $T > 0$, $SSum(X, 0, T) = $ `FALSE`
    - For $T > 0$ and $n > 0$, $SSum(X, n, T) =$
        - if $X[n] \leq T$
            - $SSum(X, n-1, T)$ or $SSum(X, n-1, T-X[n])$
        - else
            - $SSum(X, n-1, T)$

```py
def SubsetSum(X, n, T):

    S = [n+1][T+1]

    for i in range(0, n+1):
        S[i, 0] = True

    for j in range(1, T+1):
        S[0, j] = False

    for i in range(1, n+1):
        for j in range(1, T+1):
            if X[i] <= j:
                S[i, j] = S[i-1, j] or S[i-1, j - X[i]]
            else:
                S[i, j] = S[i - 1, j]

    return S[n, T]
```
- $\mathcal{O}(nT)$ time
- $\mathcal{O}(nT)$ space, but can be reduced to $\mathcal{O}(T)$ space by storing only the last two columns of $S$.

```
ex.
X = [2, 3, 7, 8, 10]
T = 11
expected: TRUE (11 = 3 + 8)

Initial setup for S[][]

    0   1   2   3   4   5   6   7   8   9   10  11
    T   F   F   F   F   F   F   F   F   F   F   F
2   T   -   -   -   -   -   -   -   -   -   -   -
3   T   -   -   -   -   -   -   -   -   -   -   -
7   T   -   -   -   -   -   -   -   -   -   -   -
8   T   -   -   -   -   -   -   -   -   -   -   -
10  T   -   -   -   -   -   -   -   -   -   -   -


First iteration:
Compare X[i] = 2 with 1, 2 is not <= 1, inherit row above

    0   1   2   3   4   5   6   7   8   9   10  11
    T   F   F   F   F   F   F   F   F   F   F   F
2   T   F   -   -   -   -   -   -   -   -   -   -
3   T   -   -   -   -   -   -   -   -   -   -   -
7   T   -   -   -   -   -   -   -   -   -   -   -
8   T   -   -   -   -   -   -   -   -   -   -   -
10  T   -   -   -   -   -   -   -   -   -   -   -


Second iteration:
Compare X[i] = 2 with 2, 2 <= 2, get row above OR j-X[i] in above row.
    S[i-1, j]       = F
    S[i-1, j-X[i]]  = S[i-1, j-2]   = T
    = T

    0   1   2   3   4   5   6   7   8   9   10  11
    T   F   F   F   F   F   F   F   F   F   F   F
2   T   F   T   -   -   -   -   -   -   -   -   -
3   T   -   -   -   -   -   -   -   -   -   -   -
7   T   -   -   -   -   -   -   -   -   -   -   -
8   T   -   -   -   -   -   -   -   -   -   -   -
10  T   -   -   -   -   -   -   -   -   -   -   -


Final iteration:

    0   1   2   3   4   5   6   7   8   9   10  11
    T   F   F   F   F   F   F   F   F   F   F   F
2   T   F   T   F   F   F   F   F   F   F   F   F
3   T   F   T   T   F   T   F   F   F   F   F   F
7   T   F   T   T   F   T   F   T   F   T   T   F
8   T   F   T   T   F   T   F   T   T   T   T   T
10  T   F   T   T   F   T   F   T   T   T   T   T

Check bottom right value = TRUE
```
- Intuition
    - The numbers along the top represent the "target" value you want the numbers in the set to sum up to.
    - When filling out the first row after the initial setup, you set `TRUE` or `FALSE` based on whether or not you can get the target value with just the first number in the set. This can only happen if the numbers are equal, so there can only be one `TRUE` in this row (aside from the first column from the initial setup).
    - From row 2 onwards, the numbers you can use to reach the target value are the ones at $X[i]$ and above.
        - If the value at $X[i]$ is $>$ the target value, the current number cannot be used as part of the sum so it solely depends on if the previous numbers can reach the target value, which is why the `TRUE` or `FALSE` is inherited from the row above.
        - If the value at $X[i]$ is $<=$ the target value, you subtract $X[i]$ from the target value and see if the previous numbers can sum up to the remaining amount. This is the purpose of checking position $j - X[i]$ in the previous row.

```py
# Prints the exact values that make the subset sum
def PrintSubsetSum(X, n, T):
    SubsetSum(X, n, T)

    if S[n, T] == FALSE:
        return

    i = n
    j = T

    while j > 0:
        if S[i - 1, j] == True:
            i -= 1
        else:
            print(X[i])
            j -= X[i]
            i -= 1
```
```
ex.

    0   1   2   3   4   5   6   7   8   9   10  11
    T   F   F   F   F   F   F   F   F   F   F   F
2   T*  F   T   F   F   F   F   F   F   F   F   F
3   T   F   T   T*  F   T   F   F   F   F   F   F
7   T   F   T   T*  F   T   F   T   F   T   T   F
8   T   F   T   T   F   T   F   T   T   T   T   T*
10  T   F   T   T   F   T   F   T   T   T   T   T*

Column jumping happens at X[i] = 8 and 3.
```

### Knapsack problem
- A generalization of subset sum.
- Given two arrays $V$ and $W$ of length $n$, where $V$ contains values and $W$ contains the weight of each value, calculate the highest total value without the total weight exceeding a number $T$.
- Lemma for $Kn(W, V, n, T)$:
    - For every $W$ and $V$, $Kn(W, V, n, 0) = 0$
    - For $T > 0$, $Kn(W, V, 0, T) = 0$
    - For $T > 0$ and $n > 0$, $Kn(W, V, n, T) = $
        ```js
        if W[n] <= T :
            // do not include the last element in V
            let a = Kn(W, V, n-1, T)

            // include the last element in V
            let b = Kn(W, V, n-1, T-W[n]) + V[n]

            // return whichever gives the highest value
            return max(a, b);
        
        else :
            // weight would go over T, do not include
            return Kn(W, V, n-1, T)
        ```
```py
def KnapSack(W: list, V: list, n: int, T: int):
    
    K = [n+1][T+1]

    for i in range(0, n+1):
        K[i, 0] = 0

    for j in range(1, T+1):
        K[0, j] = 0

    for i in range(1, n+1):
        for j in range(1, T+1):
            if W[i] <= j:
                K[i, j] = max(K[i-1, j], K[i-1, j-W[i]] + V[i])
            else:
                K[i, j] = K[i-1, j]

    return K[n, T]
```
- $\mathcal{O}(nT)$ time