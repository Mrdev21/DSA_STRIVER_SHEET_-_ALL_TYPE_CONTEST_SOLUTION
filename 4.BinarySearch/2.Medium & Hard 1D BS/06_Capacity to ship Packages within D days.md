
# Capacity to Ship Packages Within D Days — Brute Approach

## 🧠 Intuition (Hinglish)
Brute force me hum ship capacity `cap` ko small se large sab try karenge (for example `1..sum(weights)`), aur har `cap` ke liye dekhenge kitne days lagenge (greedy left-to-right: ek day me jitna ho sake ek ship bhar dena).  
Jab kisi `cap` ke liye days <= D mil jaaye to wahi smallest valid capacity return.  
Correct hai lekin bahut slow if weights sum bada ho.

---

## 📝 Dry Run
weights = [1,2,3,4,5,6,7,8,9,10], D = 5  
sum = 55, try cap from 1..55 until valid found → expensive.  
(Example only to show brute idea; skip step-by-step because range huge.)

---

## 💻 Code
```java
// Brute: try every capacity from 1..sum(weights) (VERY SLOW in practice)
public static int shipWithinDaysBrute(int[] weights, int D) {
    long sum = 0;
    for (int w : weights) sum += w;
    for (int cap = 1; cap <= sum; cap++) {
        if (daysNeeded(weights, cap) <= D) return cap;
    }
    return (int)sum; // fallback
}

private static int daysNeeded(int[] weights, long cap) {
    int days = 1;
    long current = 0;
    for (int w : weights) {
        if (current + w <= cap) current += w;
        else {
            days++;
            current = w;
        }
    }
    return days;
}
```



# Capacity to Ship Packages Within D Days — Better Approach

## 🧠 Intuition (Hinglish)
Better than naive brute: lower bound for capacity can be `max(weights)` (kyunki har package ek ship me hi jana chahiye) aur upper bound = `sum(weights)`.  
Instead of starting from 1, start from `max..sum`. Still linear scan over that range — better but can still be large.  
Phir bhi `daysNeeded` check O(n) per capacity.

---

## 📝 Dry Run
weights = [3,2,2,4,1,4], D = 3  
max = 4, sum = 16  
Try cap=4 → daysNeeded = ? (simulate) → maybe >3  
Try cap=5 → ... eventually find minimal cap = 6 (example from standard problem).

---

## 💻 Code
```java
// Better: iterate capacity from maxWeight..sumWeight (may be faster than 1..sum)
// Still O((sum-max) * n) worst-case
public static int shipWithinDaysBetter(int[] weights, int D) {
    long sum = 0;
    int mx = 0;
    for (int w : weights) { sum += w; mx = Math.max(mx, w); }

    for (long cap = mx; cap <= sum; cap++) {
        if (daysNeeded(weights, cap) <= D) return (int)cap;
    }
    return (int)sum;
}
```
(Use same `daysNeeded` helper as above.)


# Capacity to Ship Packages Within D Days — Optimal Approach (Binary Search on Answer)

## 🧠 Intuition (Hinglish)
Optimal trick = **Binary Search on the answer (capacity)**.  
Observation: agar capacity `cap` se hum packages `<= D` days me ship kar sakte hain, to **har capacity >= cap** bhi valid hogi (monotonic). Isliye smallest valid capacity find karne ke liye binary search use kar sakte ho.

Range:
- low = max(weights)  // because each weight must fit
- high = sum(weights) // worst-case one day

Binary search:
- mid = (low + high) / 2  (safe form)
- compute `daysNeeded(weights, mid)`
  - if `daysNeeded <= D` → mid valid → try smaller (high = mid - 1), record ans = mid
  - else low = mid + 1

Return `ans` (or low at end).  
Time: O(n * log(sum(weights) - maxWeight)) i.e. O(n log S). Space O(1).

Edge-case: if D >= weights.length, minimal capacity = max(weights) may be answer.

---

## 📝 Dry Run
weights = [3,2,2,4,1,4], D = 3

max = 4, sum = 16  
low = 4, high = 16

mid = 10 → daysNeeded = 2 (<=3) → ans=10, high=9  
mid = 6 → daysNeeded = 3 (<=3) → ans=6, high=5  
mid = 4 → daysNeeded = 4 (>3) → low=5
mid = 5 → daysNeeded = 4 (>3) → low=6
loop end → ans = 6 → minimal capacity = **6**

(Verify: with capacity 6 days = 3; with capacity 5 days = 4)
---

## 💻 Code
```java
// Optimal: binary search on capacity; O(n log sum) time
public static int shipWithinDaysOptimal(int[] weights, int D) {
    long sum = 0;
    int mx = 0;
    for (int w : weights) {
        sum += w;
        mx = Math.max(mx, w);
    }

    long low = mx, high = sum;
    long ans = sum;

    while (low <= high) {
        long mid = low + (high - low) / 2;
        if (daysNeeded(weights, mid) <= D) {
            ans = mid;
            high = mid - 1; // try smaller capacity
        } else {
            low = mid + 1;  // need bigger capacity
        }
    }

    return (int) ans;
}

// helper: compute number of days required if ship capacity = cap
private static int daysNeeded(int[] weights, long cap) {
    int days = 1;
    long current = 0;
    for (int w : weights) {
        if (current + w <= cap) {
            current += w;
        } else {
            days++;
            current = w;
        }
    }
    return days;
}
```


## ✅ Complexity Summary

- Brute: O(sum(weights) * n) — impractical when sum large
    
- Better (start from max..sum): O((sum−max) * n) — still large in worst-case
    
- Optimal (Binary Search on Answer): **O(n · log S)** where S = sum(weights).  
    Space: O(1) extra (plus input).
    

## ⚠ Edge Cases / Notes

- Use `long` for sums to avoid overflow when weights large.
    
- If `D >= weights.length` then answer = `max(weights)` (each package can go in its own day).
    
- If `D == 1` then answer = `sum(weights)` (must ship all in one day).
    
- Greedy `daysNeeded` must accumulate left-to-right and start a new day when exceeding capacity.
    
- Always set `low = max(weights)`, because any capacity smaller is impossible.

