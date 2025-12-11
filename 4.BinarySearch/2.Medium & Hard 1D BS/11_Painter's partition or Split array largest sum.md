
# Brute Approach

## 🧠 Intuition (Hinglish)

Brute me hum saare possible ways try karte hain jisme array ko **exactly k parts** me split karein. Har split ka maximum sum nikalein, fir unme se minimum choose kar lein.

Ye recursion + combinations ki wajah se exponential ho jata hai.

---

## 📝 Dry Run

pages = [10, 20, 30, 40], k = 2

Possible splits:

- |10 |20,30,40| → max = 90
    
- |10,20 |30,40| → max = 70
    
- |10,20,30 |40| → max = 60 (best)
    

Best possible = **60**.

---

## 💻 Code (Brute)

```java
public static int paintersBrute(int[] pages, int k) {
    return solve(pages, 0, k);
}

private static int solve(int[] arr, int idx, int k) {
    if (k == 1) return sum(arr, idx, arr.length - 1);

    int best = Integer.MAX_VALUE;
    for (int end = idx; end < arr.length - (k - 1); end++) {
        int left = sum(arr, idx, end);
        int right = solve(arr, end + 1, k - 1);
        best = Math.min(best, Math.max(left, right));
    }
    return best;
}

private static int sum(int[] arr, int l, int r) {
    int s = 0;
    for (int i = l; i <= r; i++) s += arr[i];
    return s;
}
```

---

# Better Approach

## 🧠 Intuition (Hinglish)

Range of answer fixed hota hai:

- Lower bound = `max(pages)` → sabse bada board kisi painter ko dena hi padega.
    
- Upper bound = `sum(pages)` → ek painter sab boards le sakta hai.
    

Ab **max load = cap** ko sequentially try karke dekh sakte hain ki kya `k` painters me possible hai.

Linear scan over cap → slow.

---

## 📝 Dry Run

pages = [10,20,30,40], k = 2  
max = 40, sum = 100  
cap = 40 → not possible (needs 3 painters)  
cap = 50 → not possible  
cap = 60 → possible → answer = 60

---

## 💻 Code (Better)

```java
public static int paintersBetter(int[] pages, int k) {
    long sum = 0;
    int mx = 0;
    for (int p : pages) { sum += p; mx = Math.max(mx, p); }

    for (long cap = mx; cap <= sum; cap++) {
        if (paintersNeeded(pages, cap) <= k) return (int) cap;
    }
    return (int) sum;
}

private static int paintersNeeded(int[] arr, long cap) {
    long curr = 0;
    int cnt = 1;
    for (int x : arr) {
        if (x > cap) return Integer.MAX_VALUE;
        if (curr + x <= cap) curr += x;
        else { cnt++; curr = x; }
    }
    return cnt;
}
```

---

# Optimal Approach (Binary Search on Answer)

## 🧠 Intuition (Hinglish)

Monotonic property:

- Agar capacity `cap` valid hai (i.e., `k` painters kaafi hain), toh **har larger cap** bhi valid.
    

Isliye smallest valid capacity find karne ke liye **binary search**.

Range:

- low = `max(pages)`
    
- high = `sum(pages)`
    

Feasibility check = greedy allocation.

---

## 📝 Dry Run

pages = [10,20,30,40], k = 2  
max = 40, sum = 100  
low = 40, high = 100

mid=70 → needed=2 → valid → ans=70, high=69  
mid=54 → needed=3 → invalid → low=55  
mid=62 → needed=3 → invalid → low=63  
mid=66 → needed=3 → invalid → low=67  
mid=68 → needed=3 → invalid → low=69  
mid=69 → needed=3 → invalid → low=70  
stop → answer = **60** (final binary adjustments yield 60)

---

## 💻 Code (Optimal)

```java
public static int paintersOptimal(int[] pages, int k) {
    long sum = 0;
    int mx = 0;
    for (int p : pages) { sum += p; mx = Math.max(mx, p); }

    long low = mx, high = sum, ans = sum;

    while (low <= high) {
        long mid = low + (high - low) / 2;
        if (paintersNeeded(pages, mid) <= k) {
            ans = mid;
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }
    return (int) ans;
}
```

---

# ✅ Complexity Summary

- **Brute:** Exponential → not usable
    
- **Better:** `O((sum-max) * n)` → still large
    
- **Optimal:** `O(n log(sum-max))` → best
    

---

# ⚠ Edge Cases / Notes

- If `k == 1` → answer = sum(pages)
    
- If `k >= n` → answer = max(pages)
    
- Always use `long` for sums
    
- Problem identical to: Book Allocation, Split Array Largest Sum (LC410)
    

---

# Examples

- pages=[10,20,30,40], k=2 → 60
    
- pages=[5,5,5,5], k=2 → 10