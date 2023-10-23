## Lecture 6: Fast Multiplication

### Divide-and-conquer multiplication
```py
# x and y are n-digit numbers in base 10

def SplitMul(x, y, n):
    if n == 1:
        return x * y
    else:
        m = ceil(n/2)

        # split x into a and b
        a = x // 10^m
        b = x % 10^m

        # split y into c and d
        c = y // 10^m
        d = y % 10^m

        # multiply all pairs of a, b, c, d
        e = SplitMul(a, c, m)
        f = SplitMul(b, d, m)
        g = SplitMul(b, c, m)
        h = SplitMul(a, d, m)

        return 10^(2*m) * e + 10^m * (g + h) + f
```
- $\Omega(n^2)$ time, same as long multiplication.

### Karatsuba's Algorithm
```py
def FastMul(x, y, n):
    if n == 1:
        return x * y
    else:
        m = ceil(n/2)

        a = x // 10^m
        b = x % 10^m
        c = y // 10^m
        d = y % 10^m

        e = FastMul(a, c, m)
        f = FastMul(b, d, m)
        g = FastMul(a - b, c - d, m)

        return 10^(2*m) * e + 10^m * (e + f - g) + f
```

- $\mathcal{O}(n^{\log_2 3}) = \mathcal{O}(n^{1.58})$ time