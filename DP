# Dynamic Programming: An ICPC/Codeforces Masterclass

### For the Competitive Programmer Targeting Expert Rating and Beyond

> *“Every DP problem is just a DAG shortest/longest path problem in disguise.”*  
> — A perspective every IGM internalizes.

-----

## Table of Contents

1. [The Core DP Philosophy & Framework](#1-the-core-dp-philosophy--framework)
2. [The DP Pattern Lexicon](#2-the-dp-pattern-lexicon)
3. [Advanced CP Optimizations](#3-advanced-cp-optimizations)
4. [Competitive Programming Survival Tips & Debugging](#4-competitive-programming-survival-tips--debugging)
5. [Curated Reference & Practice Roadmap](#5-curated-reference--practice-roadmap)
6. [Live Contest Checklist](#6-live-contest-checklist)

-----

# 1. The Core DP Philosophy & Framework

## 1.1 What DP Actually Is (Beyond the Textbook Definition)

Dynamic Programming is not a formula you memorize — it is a **systematic decomposition discipline**. At its heart, every DP solution is an answer to one question:

> *“If I know the optimal answer for all smaller sub-problems, how do I compute the optimal answer for my current problem in polynomial time?”*

The two necessary and sufficient conditions are:

**1. Optimal Substructure**  
A problem exhibits optimal substructure if an optimal solution to the overall problem can be constructed from optimal solutions to its sub-problems. Formally, if $OPT(P)$ is the optimal solution to problem $P$, and $P$ decomposes into sub-problems $P_1, P_2, \ldots, P_k$, then:

$$OPT(P) = f(OPT(P_1), OPT(P_2), \ldots, OPT(P_k))$$

for some efficiently computable function $f$. The crucial point: you must verify that you cannot do better by using a *sub-optimal* solution to $P_i$ — greedy failures often stem from violated optimal substructure.

**CP-Centric Verification Test:** “If I fix the optimal choice at the current step, does the remaining sub-problem still need to be solved optimally?” If yes → Optimal Substructure holds.

**2. Overlapping Sub-problems**  
Unlike Divide & Conquer (which creates *independent* sub-problems), DP sub-problems recur. The naïve recursive tree has exponential nodes, but the number of *distinct* sub-problems is polynomial. For Fibonacci: $T(n) = T(n-1) + T(n-2)$ spawns $O(2^n)$ calls, but there are only $O(n)$ distinct arguments. This is the precise condition under which memoization yields a speedup.

-----

## 1.2 The Systematic State-Design Framework

This is the most important skill. Follow this 5-step protocol on every problem:

### Step 1: Identify the “Universe” of Sub-problems

Ask: *“What information do I need to characterize exactly where I am in the computation?”*

- For sequences: often position index $i$
- For sets: often a bitmask $S$
- For intervals: endpoints $[l, r]$
- For trees: subtree root $v$
- For strings: two pointers $i, j$

**Principle:** The state must capture all information needed to make future decisions optimally, and **nothing more**. Extra dimensions → exponential state explosion. Missing dimensions → incorrect transitions.

### Step 2: Define the Objective Function Precisely

Write down in English, then math:

$$dp[state] = \text{(optimal value of some objective for sub-problem defined by } state \text{)}$$

Be precise. “Minimum cost” — cost of what, exactly? “Number of ways” — ways to do what, with what constraints satisfied?

### Step 3: Write the Transition as a Recurrence

Identify the “last thing that happens” or “first decision made” and case-split on it. Every transition takes the form:

$$dp[\text{larger state}] = \text{opt-function over } dp[\text{smaller states}] + \text{cost}$$

where “smaller” means the sub-problem DAG is acyclic (otherwise DP breaks — handle with linear systems for cyclic expected-value DPs).

### Step 4: Establish Base Cases

Base cases are states that can be answered immediately without referencing other DP values. Common mistakes:

- Forgetting empty-string/zero-index base cases
- Off-by-one on boundary indices
- Missing infeasible states (use $+\infty$ or $-\infty$ sentinel values, not $0$)

### Step 5: Determine Computation Order and Final Answer

The recurrence defines a DAG on states. Compute in topological order: for 1D problems this is usually left-to-right; for interval DP it is by increasing interval length; for tree DP it is post-order (children before parents).

-----

## 1.3 The Backtracking → Memoization → Tabulation Pipeline

### Stage 1: Brute-Force Backtracking

Write the complete recursive search first. Do not optimize prematurely. This gives you:

- A provably correct (if slow) solution to verify against
- The natural recursion structure that reveals DP states

```
def solve(pos, ...):
    if base_case: return base_value
    best = initial_value
    for each choice c:
        best = combine(best, solve(new_pos(c), ...))
    return best
```

**Complexity:** Typically $O(k^n)$ or $O(n!)$.

### Stage 2: Top-Down Memoization

Add a cache. If `(pos, ...)` was already computed, return cached value immediately.

```
memo = {}
def solve(pos, ...):
    if (pos, ...) in memo: return memo[(pos, ...)]
    if base_case: return base_value
    best = initial_value
    for each choice c:
        best = combine(best, solve(new_pos(c), ...))
    memo[(pos, ...)] = best
    return best
```

**Complexity:** $O(\text{states} \times \text{transitions per state})$ — the first polynomial bound.

**Advantage:** Only computes *reachable* states. Correct order is guaranteed by recursion.

### Stage 3: Bottom-Up Tabulation

Invert the recursion. Fill a table in topological order. For linear DPs:

```
dp = [initial_values] * (n+1)
for i in range(1, n+1):
    for each choice c leading to state i:
        dp[i] = combine(dp[i], dp[previous(c)] + cost(c))
```

**Advantage:** No function-call overhead. Enables space optimization. Avoids recursion depth limits (critical for $n = 10^6$).

**The Key Insight:** Memoization and tabulation compute *exactly the same values* in *exactly the same structure*. Tabulation is memoization with explicit ordering and array-based storage.

-----

# 2. The DP Pattern Lexicon

## 2.1 1D & 2D Linear DP (Prefix/Suffix)

### Pattern: Maximum Subarray (Kadane’s Algorithm)

**State:**

$$dp[i] = \text{maximum sum of a contiguous subarray ending at index } i$$

**Transition:**

$$dp[i] = \max(a[i],\ dp[i-1] + a[i])$$

**Base Case:** $dp[0] = a[0]$

**Answer:** $\max_{0 \le i \le n-1} dp[i]$

**Time:** $O(n)$, **Space:** $O(1)$ (only need previous value)

**Key Insight:** At each position, either extend the previous subarray or start fresh. The optimality argument: if $dp[i-1] < 0$, including it can only decrease the sum.

-----

### Pattern: Longest Increasing Subsequence (LIS)

#### $O(n^2)$ DP

**State:**

$$dp[i] = \text{length of the longest increasing subsequence ending at index } i$$

**Transition:**

$$dp[i] = 1 + \max_{j < i,\ a[j] < a[i]} dp[j]$$

**Base Case:** $dp[i] = 1$ for all $i$ (each element is an LIS of length 1)

**Answer:** $\max_{0 \le i \le n-1} dp[i]$

**Time:** $O(n^2)$, **Space:** $O(n)$

#### $O(n \log n)$ via Patience Sorting

Maintain a `tails` array where `tails[k]` = the **smallest tail element** of any increasing subsequence of length $k+1$ seen so far.

**Invariant:** `tails` is always strictly sorted.

**Algorithm:** For each element $a[i]$:

- Binary search for the leftmost position $p$ in `tails` where $\text{tails}[p] \ge a[i]$
- Replace $\text{tails}[p] = a[i]$ (or append if $p = |\text{tails}|$)

**Answer:** $|\text{tails}|$

**Correctness Argument:** By maintaining the invariant that `tails[k]` is the minimal possible tail, we maximize the chance of extending any length-$k$ subsequence. The binary search finds the optimal position in $O(\log n)$.

**Time:** $O(n \log n)$, **Space:** $O(n)$

**LIS Reconstruction:** To recover the actual subsequence, maintain a `parent[]` array and a `pos[]` array recording which index produced each `tails` value.

**Variants:**

- **Non-decreasing LIS:** Change `lower_bound` to `upper_bound` in the binary search
- **LIS count:** Augment with BIT/Fenwick tree
- **Longest Bitonic Subsequence:** Compute LIS from left and LDS from right, combine

-----

### Pattern: 2D DP on Grids

**Problem:** Count paths / find min-cost path from $(0,0)$ to $(n-1,m-1)$ in a grid, moving only right or down.

**State:**

$$dp[i][j] = \text{min-cost/count to reach cell } (i,j)$$

**Transition (min-cost):**

$$dp[i][j] = \min(dp[i-1][j],\ dp[i][j-1]) + \text{cost}[i][j]$$

**Transition (count paths):**

$$dp[i][j] = dp[i-1][j] + dp[i][j-1]$$

**Base Cases:** $dp[0][0] = \text{cost}[0][0]$; boundary rows/columns filled with prefix sums or 1s respectively.

**Time:** $O(nm)$, **Space:** $O(nm)$ reducible to $O(m)$ via rolling rows.

-----

## 2.2 Combinatorics & Knapsack Variations

### 0/1 Knapsack

**Problem:** $n$ items, item $i$ has weight $w_i$ and value $v_i$. Capacity $W$. Each item used at most once. Maximize total value.

**State:**

$$dp[i][j] = \text{maximum value using items } 1\ldots i \text{ with capacity exactly/at-most } j$$

**Transition:**

$$dp[i][j] = \max(dp[i-1][j],\ dp[i-1][j-w_i] + v_i) \quad \text{if } j \ge w_i$$
$$dp[i][j] = dp[i-1][j] \quad \text{if } j < w_i$$

**Base Cases:** $dp[0][j] = 0$ for all $j$.

**Time:** $O(nW)$, **Space:** $O(nW)$ → reducible to $O(W)$

#### Space Optimization (1D Rolling Array)

Iterate the capacity dimension **backwards** (right to left) to prevent an item from being used twice:

```
dp = [0] * (W + 1)
for each item i:
    for j from W down to w[i]:
        dp[j] = max(dp[j], dp[j - w[i]] + v[i])
```

**Why backwards?** If we went left to right, `dp[j - w[i]]` might already reflect item $i$ being used, leading to the unbounded variant. Backwards ensures `dp[j - w[i]]` is still the value from the previous item row.

-----

### Unbounded Knapsack

Each item can be used any number of times.

**Transition:** Iterate capacity **forwards** (left to right):

```
for each item i:
    for j from w[i] to W:
        dp[j] = max(dp[j], dp[j - w[i]] + v[i])
```

Or equivalently with explicit 2D:

$$dp[i][j] = \max(dp[i-1][j],\ dp[i][j-w_i] + v_i)$$

Note the second term uses $dp[i][\cdot]$ not $dp[i-1][\cdot]$.

**Time:** $O(nW)$

-----

### Bounded Knapsack (Each item $i$ available $c_i$ times)

**Naïve:** Unroll into 0/1 with $\sum c_i$ items. Time: $O(W \sum c_i)$.

**Binary Grouping Optimization:** Split $c_i$ into groups of sizes $1, 2, 4, \ldots, 2^k, r$ where $1+2+\cdots+2^k+r = c_i$. This creates $O(\log c_i)$ virtual items that can represent any count from $0$ to $c_i$.

**Time:** $O(nW \log C)$ where $C = \max c_i$

**Monotone Deque (Sliding Window) Optimization:** Achieves $O(nW)$. Group residues modulo $w_i$ and use a sliding window maximum on each residue class. See Section 3.1 for details.

-----

### Partition DP / Counting Variations

**Coin Change (Count Ways):**

$$dp[j] = \sum_{i: w_i \le j} dp[j - w_i]$$

**Coin Change (Minimum Coins):**

$$dp[j] = \min_{i: w_i \le j}(dp[j - w_i] + 1)$$

**Key distinction:** “Count ways” with ordered vs. unordered selections:

- **Unordered (combinations):** Outer loop over items, inner over capacity
- **Ordered (permutations):** Outer loop over capacity, inner over items

-----

## 2.3 Interval / Range DP

### The Template

Interval DP operates on sub-problems defined by contiguous ranges $[l, r]$ of a sequence. The key insight: **iterate by increasing interval length**.

**General form:**

$$dp[l][r] = \min_{l \le k < r} \left( dp[l][k] + dp[k+1][r] + \text{cost}(l, r) \right)$$

**Iteration Order:**

```
for length from 2 to n:
    for l from 0 to n - length:
        r = l + length - 1
        dp[l][r] = INF
        for k from l to r-1:
            dp[l][r] = min(dp[l][r], dp[l][k] + dp[k+1][r] + cost(l,r))
```

**Why length-first?** When computing $dp[l][r]$, all sub-intervals of smaller length have already been computed.

-----

### Matrix Chain Multiplication

**Problem:** Given matrices $A_1, A_2, \ldots, A_n$ with dimensions $p_0 \times p_1, p_1 \times p_2, \ldots, p_{n-1} \times p_n$. Find the optimal parenthesization to minimize scalar multiplications.

**State:**

$$dp[l][r] = \text{minimum scalar multiplications to compute } A_l \cdot A_{l+1} \cdots A_r$$

**Transition:**

$$dp[l][r] = \min_{l \le k < r} \left( dp[l][k] + dp[k+1][r] + p_{l-1} \cdot p_k \cdot p_r \right)$$

**Base Cases:** $dp[i][i] = 0$ for all $i$ (single matrix, no multiplication needed)

**Time:** $O(n^3)$, **Space:** $O(n^2)$

This is CLRS Chapter 15’s flagship example. The optimal substructure proof: in any optimal parenthesization, the outermost split is at some $k$; the sub-problems on each side must also be optimally parenthesized.

-----

### Palindrome Partitioning (Minimum Cuts)

**State:**

$$dp[i] = \text{minimum cuts to partition } s[0\ldots i] \text{ into palindromes}$$

**Transition:**

$$dp[i] = \min_{0 \le j \le i,\ s[j\ldots i] \text{ is palindrome}} (dp[j-1] + 1)$$

**Base Case:** $dp[-1] = -1$ (empty prefix needs $-1$ cuts, so first palindrome contributes $0$ cuts)

**Precompute palindromes:** Use $O(n^2)$ DP or Manacher’s algorithm $O(n)$ to build `pal[j][i]` = true if $s[j\ldots i]$ is a palindrome.

**Time:** $O(n^2)$ with precomputation, **Space:** $O(n^2)$

-----

### Optimal BST (Knuth’s Classic)

$$dp[l][r] = \min_{l \le k \le r} \left( dp[l][k-1] + dp[k+1][r] + W(l,r) \right)$$

where $W(l,r) = \sum_{i=l}^{r} w_i$ is the sum of weights. Reducible to $O(n^2)$ via Knuth’s optimization (see Section 3.4).

-----

## 2.4 String DP

### Longest Common Subsequence (LCS)

**State:**

$$dp[i][j] = \text{length of LCS of } s[1\ldots i] \text{ and } t[1\ldots j]$$

**Transition:**

$$dp[i][j] = \begin{cases} dp[i-1][j-1] + 1 & \text{if } s[i] = t[j] \ \max(dp[i-1][j],\ dp[i][j-1]) & \text{otherwise} \end{cases}$$

**Base Cases:** $dp[0][j] = dp[i][0] = 0$

**Time:** $O(nm)$, **Space:** $O(nm)$ → $O(\min(n,m))$ with rolling rows

**LCS → LIS Connection:** For permutations, LCS reduces to LIS in $O(n \log n)$.

-----

### Edit Distance (Levenshtein Distance)

**State:**

$$dp[i][j] = \text{minimum edit operations to transform } s[1\ldots i] \text{ into } t[1\ldots j]$$

Operations: insert, delete, substitute (each costs 1).

**Transition:**

$$dp[i][j] = \begin{cases} dp[i-1][j-1] & \text{if } s[i] = t[j] \ 1 + \min(dp[i-1][j-1],\ dp[i-1][j],\ dp[i][j-1]) & \text{otherwise} \end{cases}$$

where the three cases in the $\min$ correspond to substitute, delete from $s$, insert into $s$.

**Base Cases:** $dp[i][0] = i$ (delete $i$ characters), $dp[0][j] = j$ (insert $j$ characters)

**Time:** $O(nm)$, **Space:** $O(nm)$ → $O(\min(n,m))$

-----

### Wildcard / Regex Matching

**State:**

$$dp[i][j] = \text{true if } s[1\ldots i] \text{ matches pattern } p[1\ldots j]$$

**Transition:**

$$dp[i][j] = \begin{cases} dp[i-1][j-1] & \text{if } p[j] = s[i] \text{ or } p[j] = \texttt{?} \ dp[i][j-1] \lor dp[i-1][j] & \text{if } p[j] = \texttt{*} \ \text{false} & \text{otherwise} \end{cases}$$

For `*`: $dp[i][j-1]$ means `*` matches zero characters; $dp[i-1][j]$ means `*` matches one more character (the current $s[i]$).

**Base Cases:** $dp[0][0] = \text{true}$; $dp[0][j] = \text{true}$ if $p[1\ldots j]$ are all `*`.

-----

### Longest Palindromic Subsequence

$$dp[i][j] = \text{length of LPS of } s[i\ldots j]$$

$$dp[i][j] = \begin{cases} dp[i+1][j-1] + 2 & \text{if } s[i] = s[j] \ \max(dp[i+1][j],\ dp[i][j-1]) & \text{otherwise} \end{cases}$$

Note: $\text{LPS}(s) = \text{LCS}(s, \text{reverse}(s))$, which provides an alternate $O(n^2)$ solution.

-----

## 2.5 DP on Trees

### Subtree Processing (Post-Order)

The natural DP order on trees: process all children before the parent. Every sub-problem is defined by a subtree rooted at $v$.

**Pattern:** DP on the subtree rooted at $v$, combining children’s values.

**Example: Maximum Independent Set on Tree**

$$dp[v][0] = \sum_{c \in \text{children}(v)} \max(dp[c][0], dp[c][1])$$

$$dp[v][1] = 1 + \sum_{c \in \text{children}(v)} dp[c][0]$$

- $dp[v][0]$: max independent set in subtree of $v$, where $v$ is **not** selected
- $dp[v][1]$: max independent set in subtree of $v$, where $v$ **is** selected

**Answer:** $\max(dp[\text{root}][0], dp[\text{root}][1])$

-----

### Tree Knapsack (Dependent Knapsack)

**Problem:** Select a subtree of at most $K$ nodes from a rooted tree to maximize sum of values.

**State:**

$$dp[v][k] = \text{maximum value achievable by selecting exactly } k \text{ nodes from subtree of } v, \text{ including } v$$

**Transition (DFS merge):** When merging child $c$ into subtree of $v$:

$$dp[v][k] = \max_{0 \le j \le k-1} (dp[v][k-j] + dp[c][j])$$

**Important:** Iterate $k$ from large to small to avoid using updated values. The total time is $O(nK)$ amortized if done carefully (each pair of nodes contributes to the merge at most once at their LCA).

-----

### Rerooting Technique (In-Out DP)

**Problem:** Compute some value for each node $v$ as if $v$ were the root (e.g., sum of distances to all nodes, diameter passing through $v$).

**Two Passes:**

**Pass 1 (Downward):** Root at node 1, compute $dp_down[v]$ = answer restricted to subtree of $v$.

**Pass 2 (Upward):** Compute $dp_up[v]$ = contribution from the “outside” part (the tree minus subtree of $v$).

**Combination:** $\text{ans}[v] = f(dp_down[v], dp_up[v])$

**Example: Sum of Distances from Each Node**

- $dp_down[v]$ = sum of distances from $v$ to all nodes in its subtree
- $dp_up[v]$ = sum of distances from $v$ to all nodes **not** in its subtree

Transition for downward: $dp_down[v] = \sum_{c} (dp_down[c] + \text{size}[c])$

Transition for upward (when parent $p$ is rerooted):

$$dp_up[c] = dp_up[p] + (dp_down[p] - dp_down[c] - \text{size}[c]) + (n - \text{size}[c])$$

**Time:** $O(n)$ total for both passes.

-----

### Tree Diameter

**State:** During DFS from root, for each node $v$:

$$dp[v] = \text{length of longest path from } v \text{ downward in its subtree}$$

**Transition:**

$$dp[v] = 1 + \max_{c \in \text{children}(v)} dp[c]$$

**Diameter update:** When visiting $v$, the longest path through $v$ is the sum of its two longest child paths:

$$\text{diameter} = \max\left(\text{diameter},\ dp[c_1] + dp[c_2]\right)$$

where $c_1, c_2$ are the two children with largest $dp$ values.

-----

## 2.6 Bitmask DP

### State Representation

For problems on small sets ($n \le 20$, typically $n \le 20$ for $2^n$ states), represent the subset $S \subseteq {0, 1, \ldots, n-1}$ as an integer bitmask. Bit $i$ of $S$ is 1 iff element $i$ is in $S$.

**Key operations:**

- Check if $i \in S$: `(S >> i) & 1`
- Add $i$ to $S$: `S | (1 << i)`
- Remove $i$ from $S$: `S & ~(1 << i)`
- Iterate over all subsets of $S$: `for (int T = S; T > 0; T = (T-1) & S)`

-----

### Traveling Salesman Problem (TSP)

**Problem:** Find a minimum-weight Hamiltonian cycle in a complete graph on $n$ nodes.

**State:**

$$dp[S][v] = \text{minimum cost to visit exactly the nodes in } S, \text{ starting from node } 0, \text{ ending at node } v$$

where $v \in S$ and $0 \in S$.

**Transition:**

$$dp[S][v] = \min_{u \in S \setminus {v},\ (u,v) \in E} \left( dp[S \setminus {v}][u] + w(u,v) \right)$$

**Base Case:** $dp[{0}][0] = 0$

**Answer:** $\min_{v \ne 0} \left( dp[{0,1,\ldots,n-1}][v] + w(v, 0) \right)$

**Time:** $O(2^n \cdot n^2)$, **Space:** $O(2^n \cdot n)$

-----

### Iterating Over All Submasks in $O(3^n)$

To iterate over all pairs $(S, T)$ where $T \subseteq S \subseteq {0,\ldots,n-1}$:

```cpp
for (int S = 0; S < (1 << n); S++)
    for (int T = S; T > 0; T = (T-1) & S)
        process(S, T);
```

**Why $O(3^n)$?** For each element $i$, it can be: not in $S$, in $S$ but not in $T$, or in both $S$ and $T$ — exactly 3 choices. Total iterations: $\sum_{S} 2^{|S|} = 3^n$ by binomial theorem.

-----

### Profile DP (Broken Profile DP)

For problems on 2D grids (e.g., tiling with dominoes), the bitmask represents the “profile” — the state at the boundary between processed and unprocessed cells.

**State:**

$$dp[i][\text{mask}] = \text{number of ways to tile the first } i \text{ columns such that the boundary profile is mask}$$

**Time:** $O(n \cdot 2^m \cdot m)$ where $m$ is the smaller dimension.

-----

## 2.7 Digit DP

### The Standard Template

**Problem:** Count integers in $[L, R]$ satisfying property $P$ (e.g., digit sum divisible by $k$, no two consecutive equal digits, sum of digits = some value, etc.)

**Standard reduction:** $\text{count}(L, R) = f(R) - f(L-1)$ where $f(x)$ counts integers in $[0, x]$ satisfying $P$.

**State:**

$$dp[\text{pos}][\text{state}][\text{tight}][\text{started}]$$

- $\text{pos}$: current digit position (from most significant)
- $\text{state}$: accumulated information needed to check $P$ (e.g., digit sum mod $k$, last digit, etc.)
- $\text{tight}$: boolean — are we still constrained to the prefix of $x$? If true, the current digit can be at most $x[\text{pos}]$; otherwise it can be 0–9.
- $\text{started}$: boolean — have we placed a non-zero digit yet? (Handles leading zeros)

**Transition:**

For each digit $d$ from $0$ to $(\text{tight} ? x[\text{pos}] : 9)$:

$$dp[\text{pos}][\text{state}][\text{tight}][\text{started}] \mathrel{+}= dp[\text{pos}+1][\text{next_state}][\text{tight} \land (d = x[\text{pos}])][\text{started} \lor (d > 0)]$$

**Template Code (Python-style pseudocode):**

```python
from functools import lru_cache

def count_up_to(X):
    digits = list(map(int, str(X)))
    n = len(digits)
    
    @lru_cache(maxsize=None)
    def dp(pos, state, tight, started):
        if pos == n:
            return 1 if is_valid(state, started) else 0
        
        limit = digits[pos] if tight else 9
        result = 0
        for d in range(0, limit + 1):
            new_tight = tight and (d == limit)
            new_started = started or (d > 0)
            new_state = transition(state, d, new_started)
            result += dp(pos + 1, new_state, new_tight, new_started)
        return result
    
    return dp(0, initial_state, True, False)

answer = count_up_to(R) - count_up_to(L - 1)
```

**Space:** $O(\text{pos} \times |\text{state}| \times 2 \times 2)$; the `tight` and `started` dimensions are often eliminated in practice since memoization handles them (tight=True states are visited at most once per call path).

-----

## 2.8 Expected Value & Probability DP

### Acyclic State Graphs

When the DP DAG is acyclic, expected value propagates straightforwardly:

$$E[v] = \sum_u P(v \to u) \cdot (E[u] + \text{cost}(v, u))$$

Compute in reverse topological order. Base case: $E[\text{terminal}] = 0$.

-----

### Cyclic States — Systems of Linear Equations

When transitions form cycles (e.g., “flip a fair coin until heads, how many flips expected?”), write equations and solve:

**Example:** Expected flips to get heads: Let $E$ = expected flips from any state.

$$E = \frac{1}{2} \cdot 1 + \frac{1}{2}(E + 1) \implies E = 2$$

For more complex cyclic DPs with $k$ states, write a $k \times k$ linear system and solve via Gaussian elimination in $O(k^3)$.

-----

### Probability DP on Games

**State:**

$$dp[i][j] = \text{probability of player 1 winning from state } (i, j)$$

**Transition:** Depends on whether it’s player 1’s or player 2’s turn. Player 1 maximizes, player 2 minimizes probability of player 1 winning.

-----

### Expected Value with “Restart” (Geometric Distribution Pattern)

If from state $s$ you can reach $s$ again with probability $p$ and advance with probability $1-p$:

$$E[s] = p \cdot (E[s] + c) + (1-p) \cdot c’ \implies E[s] = \frac{c’}{1-p} + \frac{pc}{1-p}$$

Identify this pattern: it always collapses to a single-variable linear equation.

-----

# 3. Advanced CP Optimizations

## 3.1 Prefix Sum / Sliding Window Optimization

### When to Apply

The transition has the form:

$$dp[i] = \min_{l(i) \le j \le r(i)} dp[j] + f(i)$$

where $[l(i), r(i)]$ is a *sliding window* — as $i$ increases, both $l(i)$ and $r(i)$ are **non-decreasing**.

### Method: Monotone Deque

Maintain a deque of candidates for the optimal $j$.

- **Pop from front:** If $j < l(i)$, it’s out of window, remove it.
- **Pop from back:** If `dp[j_back] ≥ dp[j_new]`, the new $j$ dominates (for minimization with no $j$-dependent term), remove it.
- The front of the deque is always the optimal $j$.

**Complexity reduction:** $O(n^2) \to O(n)$

**Example — Bounded Knapsack via Deque:**

Group items by residue class mod $w_i$. Within each residue class $r$:

$$dp[r + k \cdot w_i] = \max_{0 \le j \le \min(k, c_i)} \left(dp[r + (k-j) \cdot w_i] + j \cdot v_i\right)$$

This is a sliding window maximum of window size $c_i + 1$ over the residue class.

-----

## 3.2 Convex Hull Trick (CHT) & Li Chao Trees

### When to Apply

The transition has the form:

$$dp[i] = \min_{j < i} \left( b_j \cdot x_i + c_j \right)$$

where:

- $b_j$ and $c_j$ depend only on $j$ (forming a *line* $y = b_j x + c_j$)
- $x_i$ depends only on $i$ (the *query* point)

Each state $j$ defines a line; querying at $x_i$ picks the minimum.

**Complexity reduction:** $O(n^2) \to O(n)$ or $O(n \log n)$

-----

### Classical CHT (Offline, Monotone)

**When applicable:** Lines are added in order of *decreasing* slope ($b_j$ is non-increasing), and queries are in *increasing* order of $x_i$ (for min CHT).

Maintain the lower convex hull of lines. Since queries are monotone, use a pointer (amortized $O(1)$ per query).

**Algorithm:**

1. Represent each state $j$ as a line $L_j: y = b_j \cdot x + c_j$.
2. Maintain a stack of lines forming the lower convex hull.
3. When adding line $L_j$: pop the last line $L_{top}$ from the stack if $L_j$ dominates $L_{top}$ everywhere in the relevant range. Specifically, pop if the intersection of $L_{top-1}$ and $L_j$ is to the left of the intersection of $L_{top-1}$ and $L_{top}$.
4. For query $x_i$: walk the pointer forward while the next line gives a smaller value.

**Intersection of $y = m_1 x + b_1$ and $y = m_2 x + b_2$:**

$$x_{\text{intersect}} = \frac{b_2 - b_1}{m_1 - m_2}$$

-----

### Li Chao Tree (Online, Arbitrary Order)

When lines and queries arrive in arbitrary order, use a Li Chao Tree (segment tree on the $x$-axis).

**Structure:** Each node covers a range $[l, r]$ of $x$-values and stores at most one line — the “dominant” line at the midpoint.

**Insert line $L$:** At each node covering $[l, r]$ with midpoint $m$:

- If $L(m) < \text{stored}(m)$: $L$ becomes the new stored line; recurse with the old line into the child where the old line was dominant.
- Otherwise: recurse with $L$ into the child where $L$ might dominate.

**Query $x$:** Walk from root to the $x$-leaf, taking the minimum among all stored lines encountered.

**Time:** $O(\log n)$ per insert/query, **Space:** $O(n \log n)$

**Advantage over CHT:** Handles arbitrary order of slopes and queries.

-----

### Worked Example: Slope Trick / DP Optimization

**Problem:** Divide $n$ elements into groups, cost of a group $[l, r]$ is $C(l,r)$. Minimize total cost.

$$dp[i] = \min_{j < i} \left( dp[j] + C(j+1, i) \right)$$

If $C(j+1, i) = a_i \cdot b_j + c_j$ for some decomposition, CHT applies directly.

**Standard form achieved by expanding $C$:** For factory/job scheduling problems where $C(j, i) = (h_i - h_j)^2$:

$$dp[i] = \min_{j < i} \left( dp[j] + h_i^2 - 2 h_i h_j + h_j^2 \right) = h_i^2 + \min_{j < i} \left( (dp[j] + h_j^2) - 2 h_j \cdot h_i \right)$$

Lines: slope $m_j = -2h_j$, intercept $c_j = dp[j] + h_j^2$, query point $x_i = h_i$.

-----

## 3.3 Divide and Conquer Optimization

### When to Apply

The transition has the form:

$$dp[i][j] = \min_{k} \left( dp[i-1][k] + C(k, j) \right)$$

and the cost function $C$ satisfies the **quadrangle inequality (concave SMAWK condition)**:

$$C(a, c) + C(b, d) \le C(a, d) + C(b, c) \quad \text{for all } a \le b \le c \le d$$

Under this condition, the optimal split point $\text{opt}(i, j)$ — the smallest $k$ achieving the minimum — satisfies:

$$\text{opt}(i, j) \le \text{opt}(i, j+1)$$

This monotonicity of the optimal split point is the key invariant.

**Complexity reduction:** $O(n^3) \to O(n^2 \log n)$

-----

### Algorithm

For each row $i$ of the DP:

```
solve(i, lo, hi, opt_lo, opt_hi):
    if lo > hi: return
    mid = (lo + hi) / 2
    best = INF, opt = opt_lo
    for k from opt_lo to min(opt_hi, mid):
        if dp[i-1][k] + C(k, mid) < best:
            best = dp[i-1][k] + C(k, mid)
            opt = k
    dp[i][mid] = best
    solve(i, lo, mid-1, opt_lo, opt)
    solve(i, mid+1, hi, opt, opt_hi)
```

**Why correct?** By monotonicity, the optimal split for any $j < \text{mid}$ is at most $\text{opt}(\text{mid})$, and for any $j > \text{mid}$ is at least $\text{opt}(\text{mid})$.

**Time:** $O(n \log n)$ per row → $O(n^2 \log n)$ total (for $n$ rows of an $n \times n$ DP).

-----

## 3.4 Knuth’s Optimization

### When to Apply

For interval DP of the form:

$$dp[l][r] = \min_{l \le k \le r} \left( dp[l][k] + dp[k][r] + C(l, r) \right)$$

where $C$ satisfies both:

1. **Monotonicity:** $C(a, c) \le C(b, d)$ for $a \le b \le c \le d$
2. **Quadrangle inequality:** $C(a, c) + C(b, d) \le C(a, d) + C(b, c)$ for $a \le b \le c \le d$

Let $\text{opt}(l, r)$ be the optimal split point for interval $[l, r]$. Knuth proved:

$$\text{opt}(l, r-1) \le \text{opt}(l, r) \le \text{opt}(l+1, r)$$

**Complexity reduction:** $O(n^3) \to O(n^2)$

-----

### Algorithm

```cpp
for (int len = 2; len <= n; len++) {
    for (int l = 0; l + len - 1 < n; l++) {
        int r = l + len - 1;
        dp[l][r] = INF;
        for (int k = opt[l][r-1]; k <= opt[l+1][r]; k++) {
            if (dp[l][k] + dp[k][r] + C(l,r) < dp[l][r]) {
                dp[l][r] = dp[l][k] + dp[k][r] + C(l,r);
                opt[l][r] = k;
            }
        }
    }
}
```

**Why $O(n^2)$?** For fixed $l$, summing over $r$: $\sum_{r=l+1}^{n} (\text{opt}(l+1, r) - \text{opt}(l, r-1)) = O(n)$. Total across all $l$: $O(n^2)$.

**Classical Applications:** Optimal BST, Matrix Chain Multiplication (with appropriate cost), Printing Neatly (CLRS), Stone Merging.

-----

## 3.5 Matrix Exponentiation

### When to Apply

A DP recurrence of the form:

$$dp[n] = c_1 \cdot dp[n-1] + c_2 \cdot dp[n-2] + \cdots + c_k \cdot dp[n-k] + \text{const}$$

with $n$ up to $10^{18}$ and $k$ small (typically $k \le 30$).

### The Method

Express the recurrence as a matrix-vector product:

$$\begin{pmatrix} dp[n] \ dp[n-1] \ \vdots \ dp[n-k+1] \end{pmatrix} = M \cdot \begin{pmatrix} dp[n-1] \ dp[n-2] \ \vdots \ dp[n-k] \end{pmatrix}$$

where the **companion matrix** $M$ is $k \times k$:

$$M = \begin{pmatrix} c_1 & c_2 & \cdots & c_{k-1} & c_k \ 1 & 0 & \cdots & 0 & 0 \ 0 & 1 & \cdots & 0 & 0 \ \vdots & & \ddots & & \vdots \ 0 & 0 & \cdots & 1 & 0 \end{pmatrix}$$

Then:

$$\begin{pmatrix} dp[n] \ dp[n-1] \ \vdots \end{pmatrix} = M^{n-k} \cdot \begin{pmatrix} dp[k] \ dp[k-1] \ \vdots \ dp[1] \end{pmatrix}$$

Compute $M^{n-k}$ via **fast matrix exponentiation** (repeated squaring):

$$M^n = \begin{cases} I & n = 0 \ (M^{n/2})^2 & n \text{ even} \ M \cdot M^{n-1} & n \text{ odd} \end{cases}$$

**Time:** $O(k^3 \log n)$, where $k$ is the dimension of $M$.

**Applications:**

- Fibonacci for $n = 10^{18}$: $k = 2$, time $O(\log n)$
- Counting paths of length $n$ in a graph: $k = |V|$, time $O(|V|^3 \log n)$
- Tiling problems, coin change with fixed denominations and large $n$

**Important:** Add a constant term by augmenting the state vector with a 1:

$$\text{If } dp[n] = c_1 dp[n-1] + d, \quad \text{use } M = \begin{pmatrix} c_1 & d \ 0 & 1 \end{pmatrix}$$

-----

# 4. Competitive Programming Survival Tips & Debugging

## 4.1 Space Optimization: Rolling Arrays

### From $O(NM)$ to $O(M)$

When the recurrence for row $i$ only depends on row $i-1$ (and no earlier rows), you only need to store two rows simultaneously.

**Technique:** Replace `dp[i][j]` with `dp[i % 2][j]` or simply alternate between `cur[]` and `prev[]`:

```cpp
// Before optimization
for (int i = 1; i <= n; i++)
    for (int j = 0; j <= W; j++)
        dp[i][j] = max(dp[i-1][j], dp[i-1][j - w[i]] + v[i]);

// After rolling array
for (int i = 1; i <= n; i++)
    for (int j = 0; j <= W; j++)
        cur[j] = max(prev[j], (j >= w[i]) ? prev[j - w[i]] + v[i] : prev[j]);
swap(cur, prev);
```

### From $O(NM)$ to $O(N)$ (1D knapsack)

Already shown in Section 2.2. The key principle: the iteration order (forward vs. backward) encodes whether a row depends on the current or previous row’s values.

### The 3-Row Trick

For recurrences depending on rows $i-1$ and $i-2$: use indices mod 3. Generalize to mod $k$ for recurrences spanning $k$ rows.

### Important Caveat

Rolling arrays make it impossible to reconstruct the optimal solution via backtracking through the DP table. If you need reconstruction, keep the full table or maintain a separate parent-pointer array.

-----

## 4.2 Iterative vs. Recursive: When to Use Which

|Criterion           |Memoization (Top-Down)                   |Tabulation (Bottom-Up)   |
|--------------------|-----------------------------------------|-------------------------|
|Implementation Speed|Faster to code                           |Slightly more careful    |
|Unreachable States  |Skipped automatically                    |All states computed      |
|Recursion Depth     |Stack overflow at ~$10^4$–$10^5$         |No recursion, no limit   |
|Space Optimization  |Very hard                                |Easy (rolling array)     |
|Transition Ordering |Automatic                                |Must be explicit         |
|Debugging           |Easy (trace call stack)                  |Must print table manually|
|Speed Constant      |Slower (function call overhead, hash map)|Faster (array access)    |

**Rule of Thumb:**

- Use **top-down** when: state space is sparse (many unreachable states), transitions are complex and order is non-obvious, or rapid prototyping during early contest minutes.
- Use **bottom-up** when: $n$ is large enough to risk stack overflow (> $10^5$ states deep), space optimization is needed, or time constant matters for tight time limits.

**Recursion Limit Fix (Python):** `sys.setrecursionlimit(300000)` — but still prefer iterative for $n > 10^5$.

-----

## 4.3 State Space Reduction

### Identifying Redundant Dimensions

**Technique 1: Check if a dimension can be derived from others.** If you have `dp[i][j][k]` where $k = f(i, j)$ always, then $k$ is redundant. Example: In a DP where “items used so far = $i$ items from first $j$” and total items always equals the current index, the explicit count is redundant.

**Technique 2: Exchange Argument.** If the order within a dimension doesn’t matter, replace it with an aggregate (sum, max, count). Classic example: scheduling problems where the optimal order of jobs can be sorted greedily, reducing the state from “which jobs chosen” (exponential) to “first $k$ jobs in sorted order” (polynomial).

**Technique 3: Precompute a sufficient statistic.** Instead of tracking raw state, track a compressed statistic. Example: in Digit DP, instead of tracking the full digit sequence, track only (digit sum mod $k$, parity of digit count).

**Technique 4: Observe monotonicity.** If the optimal value for state $(i, j)$ is monotone in $j$ given $i$, you might only need to binary search for the critical $j$.

-----

## 4.4 Debugging DP: A Systematic Protocol

### Step 1: Verify the Recurrence on Small Cases

Before running on the full input, trace your recurrence by hand on a $2 \times 3$ or $4$-element example. Compare against brute force.

### Step 2: Print the Full DP Table

```cpp
// After filling dp[][]
for (int i = 0; i <= n; i++) {
    for (int j = 0; j <= m; j++)
        cout << dp[i][j] << " ";
    cout << "\n";
}
```

Visually inspect for: monotone rows/columns (should hold for many DPs), sentinel values leaking into final answer (uninitialized $+\infty$ cells being used), all-zero rows (base cases missing).

### Step 3: Common Bug Checklist

- **Off-by-one:** Are indices 0-based or 1-based consistently? Is the array large enough?
- **Wrong initialization:** Should initial value be $0$, $+\infty$, $-\infty$, or something else? Use `memset(dp, 0x3f, sizeof(dp))` for $+\infty$ ($\approx 1.06 \times 10^9$), or `memset(dp, 0x80, sizeof(dp))` for $-\infty$.
- **Transition direction:** Are you computing `dp[i]` from `dp[i-1]` (left to right) or `dp[i-1]` from `dp[i]` (right to left)?
- **Boundary conditions:** Does your loop ever access `dp[-1]` or `dp[n+1]`?
- **Integer overflow:** When adding costs, does the sum exceed `int` range? Use `long long` and initialize with `1e18` not `INT_MAX` (to prevent overflow in `dp[j] + cost`).
- **Modular arithmetic:** When counting solutions mod $p$, check that you never subtract without re-adding $p$ first.

### Step 4: Stress Testing

Generate random small inputs ($n \le 8$), run both your $O(2^n)$ brute-force and your DP, compare outputs. This is the fastest way to find transition bugs.

-----

# 5. Curated Reference & Practice Roadmap

## 5.1 AtCoder Educational DP Contest (A–Z)

This contest is the single best structured DP curriculum available. Below is a precise mapping:

|Problem|Title          |Concept                           |Pattern                      |
|-------|---------------|----------------------------------|-----------------------------|
|A      |Frog 1         |Min-cost path, 1 choice           |1D Linear DP                 |
|B      |Frog 2         |Min-cost path, $k$ choices        |1D Linear DP                 |
|C      |Vacation       |1D DP with exclusion constraint   |1D DP, State per day         |
|D      |Knapsack 1     |Classic 0/1 Knapsack              |Section 2.2                  |
|E      |Knapsack 2     |Knapsack with large $W$, small $V$|Swap weight/value dimensions |
|F      |LCS            |Longest Common Subsequence        |Section 2.4                  |
|G      |Longest Path   |DP on DAG                         |Topological sort + 1D DP     |
|H      |Grid 1         |Count paths in grid               |2D DP                        |
|I      |Coins          |Probability DP                    |Section 2.8                  |
|J      |Sushi          |Expected Value, cycles            |Linear equations or clever DP|
|K      |Stones         |Game Theory (Sprague-Grundy)      |Grundy values / DP           |
|L      |Deque          |Two-player game DP                |Interval DP variant          |
|M      |Candies        |Partition DP with prefix sums     |Prefix sum optimization      |
|N      |Slimes         |Interval DP (merge cost)          |Section 2.3                  |
|O      |Matching       |Bitmask DP on bipartite matching  |Section 2.6                  |
|P      |Independent Set|Tree DP                           |Section 2.5                  |
|Q      |Flowers        |LIS + DP                          |1D DP + BIT optimization     |
|R      |Walk           |Matrix exponentiation             |Section 3.5                  |
|S      |Digit Sum      |Digit DP                          |Section 2.7                  |
|T      |Permutation    |DP + prefix sums                  |Section 3.1                  |
|U      |Grouping       |Bitmask DP over subsets           |Section 2.6                  |
|V      |Subtree        |Rerooting DP                      |Section 2.5                  |
|W      |Intervals      |DP + segment tree                 |Section 3.2 (Li Chao)        |
|X      |Tower          |Sorting + DP                      |Exchange argument            |
|Y      |Grid 2         |Inclusion-exclusion + DP          |Advanced 2D DP               |
|Z      |Frog 3         |Convex Hull Trick                 |Section 3.2                  |

**Study order:** A→B→C (warm up) → D→E (knapsack) → F→H (classical) → I→J (probability) → N→L (interval) → O→P→V (bitmask + tree) → R→S→Z (advanced).

-----

## 5.2 CSES Problem Set — DP Section

The CSES DP section (17 problems) is the second-best resource. Key hurdles:

|Problem               |Core Hurdle                                                   |
|----------------------|--------------------------------------------------------------|
|Minimizing Coins      |Unbounded Knapsack baseline                                   |
|Coin Combinations I   |Ordered counting (permutations of coins)                      |
|Coin Combinations II  |Unordered counting (combinations of coins)                    |
|Removing Digits       |1D DP on digit positions                                      |
|Grid Paths            |2D DP, blocked cells                                          |
|Book Shop             |0/1 Knapsack, $O(nW)$                                         |
|Array Description     |1D DP with adjacency constraints                              |
|Counting Towers       |2D state DP (shape of top row)                                |
|Edit Distance         |Classic Edit Distance                                         |
|Rectangle Cutting     |Interval DP                                                   |
|Money Sums            |Subset-sum counting (feasible sums)                           |
|Removal Game          |Game Theory / Interval DP                                     |
|Two Sets II           |Partition into two equal-sum sets                             |
|Increasing Subsequence|LIS $O(n \log n)$                                             |
|Projects              |Weighted interval scheduling (sort by deadline, binary search)|
|Elevator Rides        |Bitmask DP (bin packing variant)                              |
|Counting Tilings      |Profile/Broken-Profile DP                                     |
|Counting Numbers      |Digit DP                                                      |

**Hardest hurdles on CSES:**

- **Counting Tilings:** Profile DP — iterate column by column, mask = which cells in current column are already filled. Extremely clean but conceptually novel.
- **Projects:** Must sort by end time, then binary search for the latest non-overlapping previous project.
- **Elevator Rides:** Bitmask DP where state also encodes how full the last elevator car is.

-----

## 5.3 CLRS Chapter 15 — Formalization Reference

CLRS Chapter 15 (“Dynamic Programming”) provides the gold-standard formal treatment. Key lessons:

**Section 15.1 (Rod Cutting):** Introduces the top-down with memoization vs. bottom-up comparison. Rigorously proves the $O(n^2)$ vs. $O(2^n)$ naive comparison. Key insight: subproblem graph must be acyclic.

**Section 15.2 (Matrix Chain):** The quintessential $O(n^3)$ interval DP. CLRS proves optimal substructure explicitly using a “cut-and-paste” argument: if the optimal parenthesization of $A_{i..j}$ splits at $k$, then the sub-parenthesizations of $A_{i..k}$ and $A_{k+1..j}$ must be individually optimal (proved by contradiction).

**Section 15.3 (Elements of DP):** The two critical properties analyzed rigorously. Note the distinction: optimal substructure varies in (a) how many sub-problems and (b) how many choices per sub-problem. Both determine the recurrence’s complexity.

**Section 15.4 (LCS):** Introduces the *direction table* for reconstruction — a technique directly applicable to all 2D string DPs. The reconstruction insight: store not just values but *which* transition achieved the optimum.

**Section 15.5 (Optimal BST):** Introduces Knuth’s quadrangle inequality implicitly (though not by name). The formal proof that $\text{opt}(i, j-1) \le \text{opt}(i,j) \le \text{opt}(i+1,j)$ is given here.

-----

## 5.4 CP-Algorithms (cp-algorithms.com) — Advanced Reference

Critical articles for the optimization techniques in Section 3:

|Topic                    |URL Path                                    |
|-------------------------|--------------------------------------------|
|CHT & Li Chao Tree       |`/geometry/convex_hull_trick`               |
|Divide & Conquer DP      |`/dynamic_programming/divide-and-conquer-dp`|
|Knuth’s Optimization     |`/dynamic_programming/knuth-optimization`   |
|SOS (Sum over Subsets) DP|`/algebra/all-submasks`                     |
|Matrix Exponentiation    |`/algebra/matrix-exp`                       |
|Digit DP                 |`/dynamic_programming/digit-dp`             |
|Profile DP (Tilings)     |`/dynamic_programming/profile-dynamics`     |

The cp-algorithms articles are precise and contain full implementation details. The SOS DP article is particularly important for inclusion-exclusion problems:

$$f[S] = \sum_{T \subseteq S} a[T]$$

Computed in $O(n \cdot 2^n)$ via:

```cpp
for (int i = 0; i < n; i++)
    for (int S = 0; S < (1 << n); S++)
        if (S >> i & 1)
            f[S] += f[S ^ (1 << i)];
```

-----

# 6. Live Contest Checklist

When you suspect a problem requires DP, ask yourself these questions in order:

-----

### 🔍 Phase 1: Problem Recognition

- [ ] **Does the problem ask for an optimum (min/max) or a count over an exponential search space?** If yes, strong DP signal.
- [ ] **Does a greedy approach fail?** (Try a small counterexample.) If greedy fails → likely DP.
- [ ] **Can I identify a natural ordering or decomposition** (positions, intervals, subsets, prefixes) that breaks the problem into smaller independent pieces?
- [ ] **Does the optimal answer for the full problem depend on optimal answers for smaller inputs?** (Optimal Substructure check)

-----

### 🧠 Phase 2: State Design

- [ ] **What is the minimum information I need to characterize a sub-problem?** List all dimensions.
- [ ] **Can I eliminate any dimension?** (Is it derivable from others, or always has a fixed relationship?)
- [ ] **What is the total state count?** Estimate: $\prod_i (\text{size of dimension}_i)$. Is it $\le 10^7$–$10^8$?
- [ ] **Is my state definition unambiguous?** Write it in one precise English sentence.

-----

### ⚙️ Phase 3: Transition Design

- [ ] **What is the “last step” or “first decision”?** Case-split on all possibilities.
- [ ] **Is the transition DAG acyclic?** (For expected value problems: if cyclic, set up linear equations)
- [ ] **What are my base cases?** Did I handle all boundary conditions?
- [ ] **What is the transition complexity?** $O(1)$? $O(n)$? If $O(n)$, can I apply an optimization?

-----

### ⚡ Phase 4: Complexity Check

- [ ] **Total time = (number of states) × (transition time per state). Is it within the time limit** (roughly $10^8$ operations/second)?
- [ ] **If too slow, which optimization applies?**
  - Transition is a range min/max with sliding window → Monotone Deque $O(n)$
  - Transition has linear form $dp[j] + b_j \cdot x_i$ → CHT / Li Chao $O(n \log n)$
  - Interval DP with monotone split point → D&C Optimization $O(n^2 \log n)$
  - Interval DP with quadrangle inequality → Knuth $O(n^2)$
  - Linear recurrence with $n = 10^{18}$ → Matrix Exponentiation $O(k^3 \log n)$
- [ ] **Can I reduce space** with a rolling array?

-----

### 🐛 Phase 5: Debugging Protocol

- [ ] **Does my answer match brute force on $n = 3, 4, 5$?**
- [ ] **Printed the DP table for a small example?**
- [ ] **Are sentinels ($+\infty$, $-\infty$) initialized correctly and not overflowing when added to costs?**
- [ ] **Is the final answer extracted from the correct state(s)?**
- [ ] **For counting DPs: using modular arithmetic consistently** (no missed mod operations)?

-----

### 🎯 Final Sanity Checks

- [ ] **Is there a simpler pattern I missed?** (e.g., this “hard DP” is actually just Kadane’s)
- [ ] **Am I overcounting / undercounting in combinatorial DPs?** (Ordered vs. unordered selections)
- [ ] **For string DPs: are indices 0-based or 1-based, consistently?**
- [ ] **For tree DPs: did I handle the root correctly?** (No parent for root — base case)
- [ ] **For Digit DPs: did I handle leading zeros?** (`started` flag)

-----

*“The expert has failed more times than the beginner has even tried.” — The path to IGM is paved with wrong DPs, debugged carefully, understood deeply.*

-----

**Document compiled for ICPC/Codeforces preparation. Mathematical content reflects standard competitive programming curriculum. Cross-referenced with CLRS 4th Edition, cp-algorithms.com, AtCoder Educational DP Contest, and CSES Problem Set.**