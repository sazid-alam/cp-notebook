# D. Round Subset (Codeforces 837D)

### 🔗 Problem
- **Name:** D. Round Subset  
- **Link / Source:** https://codeforces.com/contest/837/problem/D  
- **Tags:** dp, knapsack, optimization, trailing-zeros

---

### 💡 Key Idea
- You have `n` numbers (flowers), each with powers of 2 and 5.  
- Goal: pick exactly `k` numbers to maximize the number of trailing zeros in their product.  
- Trailing zeros = `min(total_2s, total_5s)` for the chosen numbers.  
- **Trick:** Fix total number of 5s and then maximize total 2s — classic knapsack-style DP.

---

### 🧩 Pattern
- **DP State:** `dp[five][j]` = maximum number of 2s we can get by choosing exactly `j` numbers using total of `five` 5s.  
- **Base case:** `dp[0][0] = 0` → choosing 0 numbers uses 0 5s and gives 0 2s.  
- **Transition (for each number i):**
  - Let `v[i].ff = 2s`, `v[i].ss = 5s`.
  - Skip: `dp[five][j]` unchanged.  
  - Take: `dp[five][j] = max(dp[five][j], dp[five - v[i].ss][j-1] + v[i].ff)`  
    (only if `five >= v[i].ss` and `dp[five - v[i].ss][j-1] != -1`)  

- **Loop order:**  
  - `i = 1..n` → each number  
  - `five = MAX..0` → backward to avoid using the current number multiple times  
  - `j = k..1` → backward to avoid using the current number multiple times  
  - Backward loops ensure each number is counted at most once.

---

### 📝 Pseudocode
```cpp
dp[0][0] = 0
for i = 1..n:  // each number
    for five = MAX..0:
        for j = k..1:
            if five >= v[i].ss and dp[five - v[i].ss][j-1] != -1:
                dp[five][j] = max(dp[five][j], dp[five - v[i].ss][j-1] + v[i].ff)

// compute answer
ans = 0
for five = 0..MAX:
    if dp[five][k] != -1:
        ans = max(ans, min(five, dp[five][k]))
print(ans)
```

---

### ⚠️ Notes & Mistakes
- ❌ **Initial mistake:** Didn’t recognize it as a knapsack problem at first; tried solving like a normal DP without fixing 5s.  
- ❌ **Base case issues:** Had some mistakes with initializing `dp[0][0]`.  
- ❌ **Loop order mistakes:** Forward loops for `j` and `five` caused double-counting the same number.  
- ✅ **Correction:** Fix 5s as one dimension, then maximize 2s; use backward loops for `j` and `five` to prevent duplicates.  
- 💡 **Learning:**  
  - For “choose exactly k items” problems, backward loops prevent double-counting.  
  - When optimizing a function of multiple factors (2s & 5s), fixing one dimension and optimizing the other is a useful trick.  
- ⏱️ **Complexity:** O(n * MAX * k), where MAX is total possible 5s.

---
