

---

## Problem variants assumed
1. **Integer Nth-root (floor)**: Given integer `A >= 0` and integer `n >= 1`, find largest integer `x` such that `x^n <= A` (i.e. `floor(A^(1/n))`).  
2. **Real Nth-root (precision)**: Given real `A >= 0` and integer `n >= 1`, find `x` such that `x^n ≈ A` within given `eps` (e.g. `1e-6`).

We'll give Brute/Better/Optimal approaches for both where relevant.

---

# Integer Nth Root — Brute Approach

## 🧠 Intuition (Hinglish)
Brute force me hum `x = 0..` systematically check karenge aur jaisi hi `x^n` target `A` se zyada ho jaaye, previous `x-1` hi answer hoga. Simple but slow — O(A^(1/n)) steps.

> Note: ye approach sirf chhote `A` ke liye feasible hai.

---

## 📝 Dry Run
A = 30, n = 3 (cube root)
Try x:
- x=0 → 0^3=0 <=30  
- x=1 → 1 ≤30  
- x=2 → 8 ≤30  
- x=3 → 27 ≤30  
- x=4 → 64 >30 → stop → ans = 3

---

## 💻 Code
```java
// Brute: O(A^(1/n)) time, stops when x^n > A
public static long nthRootBrute(long A, int n) {
    long x = 0;
    while (true) {
        long p = 1;
        for (int i = 0; i < n; i++) {
            p *= x;
            if (p > A) break; // early stop to avoid unnecessary work
        }
        if (p > A) return x - 1;
        x++;
        // caution: infinite loop risk if A huge; brute not recommended for large A
    }
}
```

---

# Integer Nth Root — Better Approach (Binary Search on integers)

## 🧠 Intuition (Hinglish)
Sorted idea: `x^n` monotonic increasing in `x (x ≥ 0)`. Toh answer `x` guaranteed in `[0, max(1,A)]`. Binary search se mid choose karo, compute `mid^n`.  
Agar `mid^n <= A` → candidate, move right (low = mid+1). Else move left (high = mid-1). Finally `high` will hold floor root.  
Careful: compute `mid^n` with overflow check — do fast-multiply and stop if exceeds `A`.

Time: O(log A * n) (power cost), space O(1).

---

## 📝 Dry Run
A = 30, n = 3
low=0, high=30
mid=15 -> 15^3 huge >30 -> high=14
mid=7 -> 343>30 -> high=6
mid=3 -> 27<=30 -> ansCandidate=3, low=4
mid=5 -> 125>30 -> high=4
mid=4 -> 64>30 -> high=3
stop -> high=3 -> answer 3

---

## 💻 Code
```java
// Better: integer binary search with overflow-safe power check
public static long nthRootIntegerBinary(long A, int n) {
    long low = 0, high = Math.max(1L, A);
    long ans = 0;

    while (low <= high) {
        long mid = low + (high - low) / 2;
        if (powerLeq(mid, n, A)) { // mid^n <= A ?
            ans = mid;
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return ans;
}

// helper: returns true if base^exp <= limit (without overflow)
private static boolean powerLeq(long base, int exp, long limit) {
    long result = 1;
    for (int i = 0; i < exp; i++) {
        // if result * base > limit -> overflow or exceed
        if (result > limit / base) return false;
        result *= base;
    }
    return result <= limit;
}
```

---

# Integer Nth Root — Optimal Approach (Binary Search with fast exponent / improved bounds)

## 🧠 Intuition (Hinglish)
Same binary-search idea — but optimize:
- Upper bound can be `min(A, 1<<((64/n)+1))` for speed, but `high = Math.max(1,A)` is safe.
- Use exponentiation by squaring (fast power) with overflow detection to reduce cost from O(n) per check to O(log n) multiplications.
- Still overall O(log A * log n) multiplications.

This is the recommended integer method.

---

## 📝 Dry Run
Same as previous dry-run; power check uses exponentiation by squaring but logical flow identical.

---

## 💻 Code
```java
// Optimal integer nth root with fast exponentiation and overflow detection
public static long nthRootIntegerFast(long A, int n) {
    long low = 0, high = Math.max(1L, A);
    long ans = 0;

    while (low <= high) {
        long mid = low + (high - low) / 2;
        if (fastPowLeq(mid, n, A)) {
            ans = mid;
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return ans;
}

// fast exponentiation with overflow/limit check: returns true if base^exp <= limit
private static boolean fastPowLeq(long base, int exp, long limit) {
    long result = 1;
    long b = base;
    int e = exp;
    while (e > 0) {
        if ((e & 1) == 1) {
            // result *= b with overflow check
            if (result != 0 && b > limit / result) return false;
            result *= b;
        }
        e >>= 1;
        if (e > 0) { // prepare b = b*b for next loop; check overflow
            if (b != 0 && b > limit / b) {
                b = limit + 1; // force future multiplications to exceed limit
            } else {
                b *= b;
            }
        }
        if (result > limit) return false;
    }
    return result <= limit;
}
```

---

# Real Nth Root (floating) — Brute Approach

## 🧠 Intuition (Hinglish)
Brute me agar real root chahiye to you can iterate small steps (e.g., step = 1e-3) from 0 upwards and check `x^n` — bahut slow and inaccurate. Not recommended.

---

## 📝 Dry Run
A = 27.0, n=3, step=0.1
x=0→... x=3.0 gets 27 -> approx found but inefficient and imprecise.

---

## 💻 Code
```java
// Brute (not recommended) - stepping approach (very slow)
public static double nthRootRealBrute(double A, int n, double step) {
    double x = 0.0;
    while (x * x <= A) { // naive stopping condition for illustration; not precise
        double p = Math.pow(x, n);
        if (Math.abs(p - A) <= step) return x;
        x += step;
    }
    return x - step;
}
```

---

# Real Nth Root — Better / Optimal: Binary Search on Reals (with precision)

## 🧠 Intuition (Hinglish)
Monotonicity: for `x >= 0`, `x^n` increases with `x`. So do binary search on `x` in interval:
- If `A >= 1` → search in `[0, A]`
- If `A < 1` → search in `[0, 1]` (because nth-root of small numbers less than 1 is larger than A sometimes; safe bound is `[0, max(1,A)]`)

Stop when `high - low <= eps` (precision), or `|mid^n - A| <= eps`.

Time: O(log(range/eps) * cost(pow)). Use `Math.pow(mid, n)` or fast pow double.

---

## 📝 Dry Run
A = 27.0, n = 3, eps = 1e-6
low=0, high=27
mid=13.5 -> 13.5^3 huge -> high=13.5
... converge to 3.0 within eps.

---

## 💻 Code (double precision)
```java
// Binary search for real nth root with precision eps
public static double nthRootRealBinary(double A, int n, double eps) {
    if (A < 0) throw new IllegalArgumentException("Negative A with non-integer n not supported");
    double low = 0.0;
    double high = Math.max(1.0, A); // works for A in (0,1) too

    while (high - low > eps) {
        double mid = low + (high - low) / 2.0;
        double p = Math.pow(mid, n);
        if (p == A) return mid;
        if (p < A) low = mid;
        else high = mid;
    }
    return low; // or (low+high)/2 for slightly better
}
```

---

# Real Nth Root — Alternative: Binary Search on logs (stable for big n)

## 🧠 Intuition (Hinglish)
For large `n` or very large `A`, computing `mid^n` may overflow or lose precision. Use logs:
- Want `mid = exp( ln(A) / n )`.
- For binary search comparisons, compare `n * ln(mid)` vs `ln(A)` instead of `mid^n` vs `A`.
- Careful about domain: `mid > 0` required.

This is numerically stable for some ranges.

---

## 💻 Code (using logs)
```java
// Using logs for comparison to avoid overflow: compare n*ln(mid) with ln(A)
public static double nthRootRealBinaryLog(double A, int n, double eps) {
    if (A < 0) throw new IllegalArgumentException("Negative A not supported here");
    double low = 0.0, high = Math.max(1.0, A);
    double lnA = Math.log(A == 0 ? 1e-300 : A); // guard A==0

    while (high - low > eps) {
        double mid = low + (high - low) / 2.0;
        if (mid <= 0) {
            low = mid;
            continue;
        }
        double cmp = n * Math.log(mid); // ln(mid^n)
        if (Math.abs(cmp - lnA) <= 1e-12) return mid;
        if (cmp < lnA) low = mid;
        else high = mid;
    }
    return (low + high) / 2.0;
}
```

---

# Edge-cases & Notes (Must-read)

- **Zero and one**: nthRoot(0, n) = 0, nthRoot(1, n) = 1.
- **A < 1 (fraction)**: set high = 1.0 for real-root search, because e.g., 0.125^(1/3) = 0.5.
- **Negative A**:
  - If `n` is odd integer → real root exists (negative). Can adapt code to handle negatives by operating on absolute value and sign.
  - If `n` even and we want real root → negative A invalid (complex result).
- **Overflow**: for integer methods always check multiplication against `limit` to avoid overflow.
- **Precision**: choose `eps` depending on required digits (1e-6 for 6 decimal places). For integer root problem, don't use `double` rounding — prefer integer binary search.
- **Performance**: integer fastPowLeq uses O(log n) per check, so overall O(log A * log n) multiplications — very efficient.
- **Newton-Raphson**: faster convergence (quadratic) — good alternative, but user asked binary search so we gave BS implementations. If chaho, Newton method note/code bhi de dunga.

---

# Quick helper summary (recommended functions)
- `nthRootIntegerFast(A, n)` — reliable integer floor nth-root.
- `nthRootRealBinary(A, n, 1e-6)` — reliable real nth-root with EPS.
- `fastPowLeq(base, exp, limit)` — helper to avoid overflow.

---

If chaho to main:
- add **negative A + odd n** handling examples,  
- or **Newton-Raphson** implementation and convergence comparison,  
- or produce **unit tests** for these functions (JUnit style) so tum local run karke verify kar sako.  

Kaunsa add karun next?  
