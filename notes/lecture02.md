## Lecture 2: String Search

### π array
- integer array, same length as pattern $P$
- each $\pi[j]$ = length of the longest proper border of the substring $P[1..j]$.
```
pattern = ABAABAAAAB

   j    1   2   3   4   5   6   7   8   9   10
P[j]    A   B   A   A   B   A   A   A   A   B
π[j]    0   0   1   1   2   3   4   1   1   2
```
- Can be constructed in $\mathcal{O}(m)$ time, where $m$ is length of $P$.
```py
# arrays are 1-indexed

def ConstructPi(P):
    pi = int[len(P)] {0}        # init all values to 0

    pi[1] = 0                   # first element is always 0
    b = 0                       # counter for border length

    for i in range(2, len(P)):  # iterate until end
        while b > 0 and P[b+1] != P[i]:
            b = pi[b]
        if P[b+1] == P[i]:
            b += 1
        pi[i] = b
```

### KMP Algorithm
- $\mathcal{O}(n+m)$ time
    - $\mathcal{O}(m)$ to construct $\pi$ table
    - $\mathcal{O}(n)$ to run KMP algorithm against string $T$.
- $\mathcal{O}(m)$ extra space
```py
# arrays are 1-indexed

def KMP(str T, str P, int[] pi):

    i = 1       # index in T
    m = 0       # number of characters matched

    while i <= len(T) - len(P) + 1:
        while m < len(P) and T[i+m] == P[m+1]:
            m += 1

        if m == len(P):             # match found
            return i

        if m > 0:                   # partial match
            i += (m - pi[m])
            m = pi[m]
        else:                       # no partial match, move index forward
            i += 1
```


### Karp-Rabin Fingerprints
- Choose a prime number $q$.
- Choose a number $r \in [2..q)$ at random.
- $T$ is a string of numbers.
- The Karp-Rabin fingerprint of a substring starting at index $i$ with length $l$ is:
    - $\Phi(i, i+l) = (Σ_{k=1}^{l} T[i+k] * r^{l-k}) \mod q$
```
T = 0102121

i       1   2   3   4   5   6   7
T[i]    0   1   0   2   1   2   1
                i           j

Suppose i = 3 and length = 3
Substring: 212

Suppose q = 13
Suppose r = 10

Phi(i, i+3) = (2 * 10^2) + (2 * 10^1) + (2 * 10^0) = 212 mod 13 = 4
```
- Two identical substrings have the same KR fingerprint.
- Most times, two different substrings have different KR fingerprints.
    - False positive is possible if different substrings have the same KR fingerprint.
- For any $i \leq j \leq k$, if we have any two fingerprints in $\{\Phi(i,j), \Phi(j,k), \Phi(i,k)\}$, then we can compute the third:
    - $\Phi(i,k) = \Phi(i,j) * r^{k-j} + \Phi(j,k) \mod q$
    - $\Phi(i,j) = (\Phi(i,k) - \Phi(j,k)) / r^{k-j}  \mod q$
    - $\Phi(j,k) = \Phi(i,k) - \Phi(i,j) * r^{k-j}  \mod q$
    - Each can be evaluated in $\mathcal{O}(\log n)$ time using exponentiation by squaring.

```
T = 778548784372
   i      j    k

L = Phi(i, j) = 7785487
R = Phi(j, k) = 84372
C = Phi(i, k) = 778548784372

Given L and R, compute C:
C = L * 10^5 + R

Given L and C, compute R:
R = C - L * 10^5
```

### Karp-Rabin String Matching
- With checks, runs in $\mathcal{O}(nm)$ time
- Without checks, runs in $\mathcal{O}(n+m)$ time
- Could result in false positives.
- A sufficiently large $q$ can make the probability of collision smaller.
```py
def KR_Search(int[n] T, int[m] P):
    '''Precompute r^k mod q for all k in[0, m]'''
    h = phi_P(0, m)                 # O(m) time
    for i in range(0, n - m):
        h2 = phi_T(i, i + m)        # O(1) time
        if h == h2:
            '''optional checking'''
            return i + 1
```