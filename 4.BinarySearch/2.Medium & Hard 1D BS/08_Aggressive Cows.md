


**Problem (short):** Given `n` stall positions `stalls[]` (distinct integers) and `k` cows, place the cows in stalls so that the minimum distance between any two cows is maximized. Return that maximum minimum distance.


---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute force me hum **sab possible k-stall combinations** try karenge, har combination ke liye compute karenge pairwise distances aur uska minimum leke overall maximum choose karenge. Yeh correct hai but combinatorial explosion hogi (`C(n,k)` combos) — sirf chote n & k ke liye feasible.

---

## 📝 Dry Run

stalls = [1,2,8,4,9], k = 3

- sort stalls -> [1,2,4,8,9]
    
- choose all 3-stall combos:
    
    - (1,2,4) -> min dist = 1
        
    - (1,2,8) -> min dist = 1
        
    - (1,2,9) -> min dist = 1
        
    - (1,4,8) -> min dist = 3
        
    - (1,4,9) -> min dist = 3
        
    - (1,8,9) -> min dist = 1
        
    - (2,4,8) -> min dist = 2
        
    - (2,4,9) -> min dist = 2
        
    - (2,8,9) -> min dist = 1
        
    - (4,8,9) -> min dist = 1
        
- maximum of mins = 3 → answer = 3
    

(Brute shows correctness but is impractical for large n)

---

## 💻 Code (Java)

```java
// Brute: generate all combinations of k stalls (n choose k)
// Compute min pairwise distance for each combo, track maximum.
import java.util.*;

public static int aggressiveCowsBrute(int[] stalls, int k) {
    Arrays.sort(stalls);
    int n = stalls.length;
    int[] pick = new int[k];
    return combMaxMin(stalls, n, k, 0, 0, pick);
}

private static int combMaxMin(int[] stalls, int n, int k, int start, int depth, int[] pick) {
    if (depth == k) {
        int minDist = Integer.MAX_VALUE;
        for (int i = 1; i < k; i++) {
            minDist = Math.min(minDist, Math.abs(pick[i] - pick[i-1]));
        }
        return minDist == Integer.MAX_VALUE ? 0 : minDist;
    }
    int best = 0;
    for (int i = start; i <= n - (k - depth); i++) {
        pick[depth] = stalls[i];
        best = Math.max(best, combMaxMin(stalls, n, k, i + 1, depth + 1, pick));
    }
    return best;
}
```

## Complexity

- Time: `O(C(n,k) * k)` (exponential)
    
- Space: `O(k)` recursion
    

---

# Better Approach

## 🧠 Intuition (Hinglish)

Brute combination wasteful hai. Ek practical approach: iterate possible distances `d` from `1..(maxPos-minPos)` and for har `d` check greedily agar `k` cows place ho sakte hain (place first cow at first stall, then next cow at next stall with distance >= d, etc.).

Yeh `d` range sequential scan karta hai — better than combinations but still may be large if coordinate range huge.

---

## 📝 Dry Run

stalls = [1,2,4,8,9], k=3

- maxPos-minPos = 8
    
- try d=1 -> greedy places 3+ cows → feasible
    
- try d=2 -> feasible
    
- try d=3 -> feasible
    
- try d=4 -> not feasible
    
- max feasible found = 3 → answer 3
    

---

## 💻 Code (Java)

```java
// Better: iterate d from 1..(max-min), check feasibility via greedy O(n) each
public static int aggressiveCowsBetter(int[] stalls, int k) {
    Arrays.sort(stalls);
    int n = stalls.length;
    int lowD = 1;
    int highD = stalls[n-1] - stalls[0];
    int best = 0;
    for (int d = lowD; d <= highD; d++) {
        if (canPlace(stalls, k, d)) best = d;
    }
    return best;
}

private static boolean canPlace(int[] stalls, int k, int d) {
    int count = 1; // place first cow at stalls[0]
    int lastPos = stalls[0];
    for (int i = 1; i < stalls.length; i++) {
        if (stalls[i] - lastPos >= d) {
            count++;
            lastPos = stalls[i];
            if (count == k) return true;
        }
    }
    return false;
}
```

## Complexity

- Time: `O((maxPos-minPos) * n)`
    
- Space: `O(1)`
    

---

# Optimal Approach (Binary Search on Distance)

## 🧠 Intuition (Hinglish)

Feasible property: agar distance `d` se `k` cows place ho sakte hain, to **har smaller distance <= d** bhi place ho sakti hai? Actually monotonic in reverse: if `d` is feasible, then any `d' < d` is also feasible. So the set of feasible `d` is a prefix `1..Dmax`. We can binary search the maximum feasible `d` using greedy `canPlace` as checker.

Range:

- low = 1
    
- high = maxPos - minPos
    

Binary search to find largest `d` for which `canPlace(stalls,k,d)` is true.

Time: `O(n log(maxPos-minPos))` — best practical.

---

## 📝 Dry Run (binary search)

stalls = [1,2,4,8,9], k = 3

- sort -> [1,2,4,8,9]
    
- low=1, high=8
    
- mid=4 -> canPlace? false
    
- low=1, high=3
    
- mid=2 -> canPlace? true -> best=2, low=3
    
- mid=3 -> canPlace? true -> best=3, low=4
    
- loop ends -> answer = 3
    

---

## 💻 Code (Java)

```java
// Optimal: binary search on distance; O(n log range)
import java.util.*;

public static int aggressiveCowsOptimal(int[] stalls, int k) {
    Arrays.sort(stalls);
    int n = stalls.length;
    int low = 1;
    int high = stalls[n-1] - stalls[0];
    int best = 0;

    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (canPlace(stalls, k, mid)) {
            best = mid;           // mid feasible -> try larger
            low = mid + 1;
        } else {
            high = mid - 1;       // mid not feasible -> try smaller
        }
    }
    return best;
}

private static boolean canPlace(int[] stalls, int k, int d) {
    int count = 1;
    int lastPos = stalls[0];
    for (int i = 1; i < stalls.length; i++) {
        if ((long)stalls[i] - lastPos >= d) {
            count++;
            lastPos = stalls[i];
            if (count == k) return true;
        }
    }
    return false;
}
```

---

# ✅ Complexity Summary

- **Brute:** `O(C(n,k) * k)` — exponential, only for tiny n,k
    
- **Better (iterating distances):** `O((maxPos-minPos) * n)` — may be large if coordinates wide
    
- **Optimal (binary search):** `O(n * log(maxPos-minPos))` — recommended
    

---

# ⚠ Edge Cases / Notes

- Sort stalls first.
    
- If `k == 1` → answer is `0` (single cow, zero min distance) or logically the max possible but commonly defined as 0. (Depending on problem interpretation; SPOJ expects max-min only when k>=2).
    
- If `k >= n` → answer = min gap between adjacent stalls after optimal placement which in practice for `k==n` means cows occupy all stalls and answer = min adjacent difference.
    
- Use `long` when subtracting coordinates if positions near integer limits.
    

---

# Examples

- `stalls=[1,2,8,4,9], k=3` → answer `3`
    
- `stalls=[0,3,4,7,10], k=3` → analyze similarly
    

---

_File placement suggestion:_ `DSA/Arrays/08_Aggressive Cows.md`

_Want me to:_ add a visual explanation, Zipped testcases, or a JUnit test file? Let me know.