

**Problem (short):** Given a sorted array of unique positive integers `arr` and an integer `k`, return the `k`-th positive integer that is missing from this array.

---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute me hum 1 se start karke har positive integer `x` check karenge agar woh array me present hai ya nahi. Agar present nahi → count++ ; jab count == k return x. Presence check har baar `O(n)` karoge (linear search) to total slow ho jayega.

## 📝 Dry Run

`arr = [2,3,4,7,11], k = 5`

- check x=1 → not present → count=1
    
- x=2 → present
    
- x=3 → present
    
- x=4 → present
    
- x=5 → not present → count=2
    
- ... continue until count=5 → answer 9
    

(Brute clearly slow if k or array values bade hon)

## 💻 Code (Java)

```java
// Brute: linear check + linear search for presence -> O(k * n)
public static int findKthPositiveBrute(int[] arr, int k) {
    int count = 0;
    int x = 1;
    while (true) {
        boolean found = false;
        for (int val : arr) {
            if (val == x) { found = true; break; }
        }
        if (!found) count++;
        if (count == k) return x;
        x++;
    }
}
```

## Complexity

- Time: `O(k * n)` (worst-case)
    
- Space: `O(1)`
    

---

# Better Approach

## 🧠 Intuition (Hinglish)

Array sorted hai and unique. Hum single pass me missing count compute kar sakte hain: jab hum `arr[i]` pe hai, number of missing before `arr[i]` = `arr[i] - (i+1)`. Toh iterate karke jab missing cumulative cross kare to answer nikal sakte ho — ye O(n).

## 📝 Dry Run

`arr=[2,3,4,7,11], k=5`

- i=0: arr[0]=2 -> missingBefore=2-(0+1)=1 (missing list [1])
    
- i=1: arr[1]=3 -> missingBefore=3-2=1
    
- i=2: arr[2]=4 -> missingBefore=4-3=1
    
- i=3: arr[3]=7 -> missingBefore=7-4=3 (total missing before index 3 is 3)
    
- i=4: arr[4]=11 -> missingBefore=11-5=6 which is >=5 -> k-th missing lies before arr[4]  
    Compute answer: arr[4] - (missingBefore - k + 1) = 11 - (6 - 5 + 1) = 9
    

## 💻 Code (Java)

```java
// Better: single pass using missing = arr[i] - (i+1) -> O(n)
public static int findKthPositiveBetter(int[] arr, int k) {
    int n = arr.length;
    for (int i = 0; i < n; i++) {
        int missing = arr[i] - (i + 1);
        if (missing >= k) {
            // k-th missing is before arr[i]
            return arr[i] - (missing - k + 1);
        }
    }
    // if not found within array, it's after last element
    return arr[n - 1] + (k - (arr[n - 1] - n));
}
```

## Complexity

- Time: `O(n)`
    
- Space: `O(1)`
    

---

# Optimal Approach

> Note: For this problem _Better_ (single pass) is already optimal in linear time, but commonly we present a **binary search** variant with `O(log n)` time. Yeh variant useful hai jab array bahut bada ho aur you want logarithmic search.

## 🧠 Intuition (Hinglish)

Define `missing(i) = arr[i] - (i+1)` = number of missing numbers <= arr[i]. `missing(i)` is non-decreasing. Use binary search to find leftmost index `idx` with `missing(idx) >= k`.

- If such `idx` exists → k-th missing lies before `arr[idx]` → answer computed from arr[idx] and missing(idx).
    
- Else (missing(last) < k) → answer = arr[last] + (k - missing(last)).
    

## 📝 Dry Run (binary search)

`arr=[2,3,4,7,11], k=5`

- missing array: [1,1,1,4,6]
    
- binary search finds idx = 4 (missing=6 >=5)
    
- answer = arr[4] - (missing(4) - k + 1) = 11 - (6 - 5 + 1) = 9
    

## 💻 Code (Java)

```java
// Binary search on index -> O(log n)
public static int findKthPositiveBinarySearch(int[] arr, int k) {
    int n = arr.length;
    int low = 0, high = n - 1;
    while (low < high) {
        int mid = low + (high - low) / 2;
        int missing = arr[mid] - (mid + 1);
        if (missing < k) low = mid + 1;
        else high = mid;
    }
    // after loop low==high
    int missingAtLow = arr[low] - (low + 1);
    if (missingAtLow >= k) {
        return arr[low] - (missingAtLow - k + 1);
    } else {
        // missing at last < k
        return arr[n - 1] + (k - (arr[n - 1] - n));
    }
}
```

## Complexity

- Time: `O(log n)`
- Space: O(1)
---

# ✅ Short Summary / Tips

- `missing(i) = arr[i] - (i+1)` is the key insight.
    
- Better (O(n)) is simplest and fast for most inputs; binary search gives O(log n) when needed.
    
- Edge cases:
    
    - If `k` is small and before first element, e.g. `arr[0] > k`, formula still works.
        
    - Use care when array length = 0 (not in original constraints) or very large values.
        

---

# Examples

- `arr=[2,3,4,7,11], k=5` → `9`
    
- `arr=[1,2,3,4], k=2` → `6` (all first 4 present, missing numbers start from 5)
    

---

_File placement suggestion:_ `DSA/Arrays/07_Kth Missing Positive Number.md`

_If you want, I can:_ add step-by-step line-by-line dry-run for the O(n) approach, or JUnit testcases, or shorten further. Let me know.