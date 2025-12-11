
Problem (short):** Given an array `pages[]` where `pages[i]` represents the number of pages in the i-th book, allocate the books to **M students** such that:

- Each student gets **contiguous** books.
- Every book must be allocated.
- Minimize the **maximum pages** assigned to any student.
Return the minimum possible value of the maximum pages.

---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute me hum **har possible maxPages (cap)** try karenge from `1 .. sum(pages)`. Phir check karenge ki kya iss `cap` ke andar books ko **M students** me divide karna possible hai (greedy allocation).

Agar possible ho jaaye → vo ek valid answer hai, aur brute me smallest valid cap return.

Slow kyunki range bohot badi hoti hai.

---

## 📝 Dry Run

pages = [12, 34, 67, 90], M = 2

- sum = 203
    
- cap = 1..203 try karna → expensive
    
- Actual valid caps around 100+ hote hain → brute not practical
    

---

## 💻 Code (Java)

```java
// Brute: try maxPages from 1..sum(pages)
public static int allocateBooksBrute(int[] pages, int M) {
    long sum = 0;
    for (int p : pages) sum += p;

    for (long cap = 1; cap <= sum; cap++) {
        if (studentsNeeded(pages, cap) <= M) return (int)cap;
    }
    return (int)sum;
}

private static int studentsNeeded(int[] pages, long cap) {
    long curr = 0;
    int students = 1;
    for (int p : pages) {
        if (p > cap) return Integer.MAX_VALUE; // cannot allocate
        if (curr + p <= cap) curr += p;
        else {
            students++;
            curr = p;
        }
    }
    return students;
}
```

## Complexity

- Time: `O(sum(pages) * n)` (huge)
    
- Space: `O(1)`
    

---

# Better Approach

## 🧠 Intuition (Hinglish)

Brute se better: maxPages ka lower bound = `max(pages)` (largest book must fit), upper bound = `sum(pages)`.

Ab sequentially try karein from `max(pages)` to `sum(pages)` and check feasibility using greedy.

Better than brute, but still linear search across large range = slow.

---

## 📝 Dry Run

pages = [12, 34, 67, 90], M=2

- max = 90
    
- sum = 203  
    Try cap = 90, 91, 92, ... until valid → eventually answer = 113
    

---

## 💻 Code (Java)

```java
// Better: try cap from max..sum
public static int allocateBooksBetter(int[] pages, int M) {
    long sum = 0;
    int mx = 0;
    for (int p : pages) { sum += p; mx = Math.max(mx, p); }

    for (long cap = mx; cap <= sum; cap++) {
        if (studentsNeeded(pages, cap) <= M) return (int)cap;
    }

    return (int)sum;
}
```

(Uses same `studentsNeeded()` helper.)

## Complexity

- Time: `O((sum-max) * n)`
    
- Space: `O(1)`
    

---

# Optimal Approach (Binary Search on Answer)

## 🧠 Intuition (Hinglish)

Typical **Binary Search on Answer** problem.

Key idea: agar `cap` pages ke andar M students me allocation possible hai → har **bigger cap** bhi possible hai. Range is monotonic.

So smallest feasible `cap` find karne ke liye binary search.

Range:

- low = max(pages)
    
- high = sum(pages)
    

Greedy check: books left→right assign karte jao jab tak cap exceed na ho, phir new student.

---

## 📝 Dry Run

pages = [12,34,67,90], M = 2

- max = 90, sum = 203
    
- low=90, high=203
    

mid=146 → students=2 (valid) → ans=146, high=145  
mid=117 → students=2 (valid) → ans=117, high=116  
mid=103 → students=3 (>2) → low=104  
mid=110 → students=2 (valid) → ans=110, high=109  
... eventually → **113**

---

## 💻 Code (Java)

```java
// Optimal: O(n log sum) binary search on max allowed pages
public static int allocateBooksOptimal(int[] pages, int M) {
    long sum = 0;
    int mx = 0;
    for (int p : pages) {
        sum += p;
        mx = Math.max(mx, p);
    }

    long low = mx, high = sum;
    long ans = sum;

    while (low <= high) {
        long mid = low + (high - low) / 2;
        if (studentsNeeded(pages, mid) <= M) {
            ans = mid;
            high = mid - 1; // try smaller
        } else {
            low = mid + 1;  // need bigger cap
        }
    }

    return (int)ans;
}

// studentsNeeded reused from above
```

---

# ✅ Complexity Summary

- **Brute:** `O(sum * n)` → unusable
    
- **Better:** `O((sum-max) * n)` → still slow
    
- **Optimal:** `O(n log(sum-max))` → recommended
    

---

# ⚠ Edge Cases / Notes

- If `M > n` → impossible (each student must take at least one book). Some problems return `-1`.
    
- If `M == 1` → result = `sum(pages)`
    
- If `M == n` → result = `max(pages)`
    
- Always use `long` for sums (pages up to 1e9 possible)
    

---

# Examples

- `pages=[12,34,67,90], M=2` → `113`
    
- `pages=[5,17,100,11], M=4` → max page = 100
    

---

_File placement suggestion:_ `DSA/Arrays/09_Book Allocation Problem.md`

If you want, I can also add: **Painter Partition version**, **Minimize Largest Sum of Subarrays (LeetCode 410)** link, or **testcases table**.