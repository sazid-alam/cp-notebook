# D. Smithing Skill (Codeforces 1989D)

### 🔗 Problem
- **Name:** D. Smithing Skill  
- **Link / Source:** https://codeforces.com/contest/1989/problem/D  
- **Tags:** knapsack, dp, greedy, optimization

---

### 💡 Key Idea
- The problem reduces to minimizing the number of operations needed to reduce a value `k` to `0`, where each operation type `i` effectively reduces the remaining amount by `(a[i] - b[i])` and costs `2` (with some special lower bound behavior).
- Recognize it as a **knapsack/coin-change-like** problem: repeatedly apply operations to decrease `k`.
- **Optimization trick:** instead of applying operations one-by-one, compute how many times (`p`) you can safely apply the chosen operation in one jump (batching) — this drastically reduces the number of states.
- Precompute, for each possible value `x`, the best operation index to use (using a greedy ordering), then use memoized recursion (map) for queries.

---

### 🧩 Pattern
- **Knapsack-style DP + greedy preselection.**  
- Precompute a `best[x]` array mapping `x` → index of the operation that is the most suitable for values ≥ `x` (using a sort by `(a - b)`), then solve queries with memoized recursion that jumps multiple operations at once.

---

### 📝 Pseudocode
```cpp
function solve(k):
    if k == 0: return 0
    if dp contains k: return dp[k]

    idx = best[min(k, MAX-1)]
    if idx == -1: return dp[k] = 0

    step = a[idx] - b[idx]
    p = (k - b[idx] - 1) / step
    if p == 0: p = 1

    new_k = k - p * step
    dp[k] = 2 * p + solve(new_k)
    return dp[k]

// Answer = sum of solve(k) for all queries
```

---

### ⚠️ Notes & Mistakes
- ❌ **Mistake:** At first, I didn’t realize it was essentially a knapsack/coin-change type problem and tried greedy approaches without batching.  
- ✅ **Correction:** Identified that we can jump multiple times with the same operation (`p`) to minimize recursion calls.  
- 💡 **Learning:** Whenever there’s a process that repeats with a fixed step size, check if you can *batch the operations* to reduce complexity.  
- ⚙️ **Implementation cautions:**  
  - Use `map` for DP since query `k` can be large/sparse.  
  - Handle case when no operation is valid (`op == -1`).  
  - Carefully compute `p` with integer division, ensure `p >= 1` so recursion progresses.  
  - Set `MAX = 1e6+5` to keep array bounds safe.  
- ⏱️ **Complexity:** O(n + MAX + distinct_dp_states). With batching, the recursion depth stays manageable.  

---
