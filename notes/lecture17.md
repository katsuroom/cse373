## NP-Hardness

### Introduction
- For _decision problems_, where the answer is YES/NO:
    - Every optimization problem can be turned into a decision problem by adding parameters.
    - Thus, optimization problems are at least as hard as their decision variants.
    - If the decision version is hard, then the optimized version is unlikely to be solvable.
- Classes of decision problems:
    - **P**: Polynomial time
        - Decision problems that can be solved in polynomial time.
    - **NP**: Nondeterministic Polynomial time
        - Decision problems that we can verify a solution for in polynomial time.

### NP-Hardness
- If a problem $X$ is harder than any problem in NP, then $X$ is NP-hard.
- Cook-Levin Theorem
    - If any $X \in $ NP can be solved in polynomial time, then we can solve the other NP-complete problems in polynomial time.

### SAT
- Boolean Satisfiability Problem (SAT) is NP-hard.
- **Input**:
    - Set $X$ of $n$ boolean variables
    - A collection $C$ of clauses (size $m$), such that each clause is a set of literals $\in x_i$ and $\bar{x_i}$
- **Output**: Whether there exists values for all variables $x_i$ such that there is at least one `true` in each clause.
- $\mathcal{O}(2^n * m * n)$
    - Current known best solution contains $2^n$, is not polynomial.

### NP-Completeness
- If a problem $X$ is NP-hard and $X \in$ NP, then $X$ is NP-complete.