
**Problem (short, Hinglish):** Tumhe ek lowercase string `s` diya hai — har substring ki **beauty** define hoti hai as  
`beauty(substring) = (max frequency of any character in substring) − (min frequency among characters that appear at least once in substring)`.  
Tumhe **sabhi substrings** ki beauties ka **sum** return karna hai.

---

# Brute Approach (Check all substrings and recount)

## 🧠 Intuition (Hinglish)

Brute me har possible substring `(i..j)` generate karo aur uske liye frequency array full recompute karo, phir `max` aur `min (non-zero)` nikaal ke `max - min` add kar do. Simple hai lekin har substring ke liye counting karne se `O(n^3)` worst-case ho jata hai (n^2 substrings × O(n) count).

---

## 📝 Dry Run

s = `"aabcb"`

- i=0,j=0: `"a"` → freq a=1 → max=1,min=1 → beauty=0
    
- i=0,j=1: `"aa"` → a=2 → beauty=0
    
- i=0,j=2: `"aab"` → a=2,b=1 → beauty=1
    
- i=0,j=3: `"aabc"` → a=2,b=1,c=1 → beauty=1
    
- i=0,j=4: `"aabcb"` → a=2,b=2,c=1 → beauty=1  
    ... sab substrings enumerate kar ke sum → final 5.
    

---

## 💻 Code (Brute)


// Brute: recompute counts for each substring O(n^3) worst-case
```java
public static long beautySumBrute(String s) {
    int n = s.length();
    long total = 0;
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            int[] freq = new int[26];
            for (int k = i; k <= j; k++) {
                freq[s.charAt(k) - 'a']++;
            }
            int max = 0, min = Integer.MAX_VALUE;
            for (int f : freq) {
                if (f > 0) {
                    max = Math.max(max, f);
                    min = Math.min(min, f);
                }
            }
            if (min != Integer.MAX_VALUE) total += (max - min);
        }
    }
    return total;
}```

---

# Better / Recommended Approach (Fix start i, expand end j, update freq incrementally)

## 🧠 Intuition (Hinglish)

Fix start index `i`. Fir `j` ko `i..n-1` expand karo aur ek rolling `freq[26]` maintain karo (har step sirf 1 char update). Har `j` par 26-size scan karke `max` aur `min (non-zero)` nikal lo aur add karo. Alphabet constant (26) hone ki wajah se complexity `O(26 · n^2)` hoti hai jo practically `O(n^2)` samjhi jaati hai — simple, fast aur interview-friendly.

---

## 📝 Dry Run

s = `"aabcb"`, i = 0:

- j=0 → freq[a]=1 → max=1,min=1 → add 0
    
- j=1 → freq[a]=2 → max=2,min=2 → add 0
    
- j=2 → freq[b]=1 → a=2,b=1 → max=2,min=1 → add 1
    
- j=3 → freq[c]=1 → a=2,b=1,c=1 → max=2,min=1 → add 1
    
- j=4 → freq[b]=2 → a=2,b=2,c=1 → max=2,min=1 → add 1  
    Repeat for other starts → total 5.
    

---

## 💻 Code (Recommended, incremental O(n^2 · 26))

```java
// Recommended approach: for each start, expand end and update freq incrementally
public static long beautySum(String s) {
    int n = s.length();
    long total = 0L;

    for (int i = 0; i < n; i++) {
        int[] freq = new int[26];
        for (int j = i; j < n; j++) {
            freq[s.charAt(j) - 'a']++;
            int max = 0;
            int min = Integer.MAX_VALUE;
            // scan 26 letters to get max and min (non-zero)
            for (int f : freq) {
                if (f > 0) {
                    max = Math.max(max, f);
                    min = Math.min(min, f);
                }
            }
            if (min != Integer.MAX_VALUE) total += (max - min);
        }
    }

    return total;
}

```

---

# ✅ Complexity Summary

- **Brute:** `O(n^3)` worst-case (naive recounting)
    
- **Recommended (incremental):** `O(26 · n^2)` ≈ **O(n²)** in practice (26 constant)
    
- **Space:** `O(1)` auxiliary (fixed 26-size array). Use `long` for accumulator.
    

---

# ⚠ Edge Cases / Notes

- Empty string → return `0`.
    
- Single character string → all substrings beauty 0 → return `0`.
    
- All same characters (e.g., `"aaaa"`) → beauty always 0 → return `0`.
    
- If alphabet wasn't fixed (Unicode), this method needs adaptation (map + extra bookkeeping) and complexity rises.
    
- Use `long` for accumulator because number of substrings `≈ n²/2` and each beauty ≤ n.
    

---

# Examples

- s = `"aabcb"` → `5`  
    (contributing substrings: `"aab"(1)`, `"aabc"(1)`, `"aabcb"(1)`, `"abcb"(1)`, `"bcb"(1)`)
    
- s = `"abc"` → `0`
    
- s = `"zzzzz"` → `0`