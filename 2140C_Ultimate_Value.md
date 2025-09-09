# C. Ultimate Value (Codeforces 2140C)

### 🔗 Problem
- **Name:** C. Ultimate Value
- **Link / Source:** [https://codeforces.com/contest/2140/problem/C](https://codeforces.com/contest/2140/problem/C)
- **Tags:** games, greedy

---

### 💡 Key Idea
- We want to maximize the final value `f(a) = cost + (a1 - a2 + a3 - a4 + ...)` after swap operations that add `(r-l)` to cost.
- Trick: when indices are involved, transform expressions into forms like `a[i] ± i` and analyze extremes (max/min).
- Both directions matter (max could be on left or right).

---

### 🧩 Pattern
- **Transform + Extremes**: reduce to finding `max - min` over transformed values, separated by parity of index.

---

### 📝 Pseudocode
```cpp
// base alternating sum
sum = a1 - a2 + a3 - a4 + ...

// compute transformed extremes
mx  = max over odd (2*a[i] + (i+1))
mn  = min over even (2*a[i] + (i+1))

mx2 = max over odd (2*a[i] - (i+1))
mn2 = min over even (2*a[i] - (i+1))

ans = max(sum,
          sum + (mx - mn),
          sum + (mx2 - mn2),
          sum + 2*((n-1)/2))  // edge-case adjustment
```

---

### ⚠️ Notes / Mistakes
- **My mistake:** initially forgot the case where the max comes from the left side.
- **Lesson:** whenever terms look like `a[i] ± i`, always check both transformations (`+i` and `-i`).
- Must design stronger **custom test cases** to reveal missing scenarios.

---
