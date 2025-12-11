
**Problem (short, Hinglish):**  
Tumhe ek lowercase string `s` diya hai. Har substring ki **beauty** define hoti hai:  
`beauty = (max frequency of any character) − (min frequency among characters that appear at least once)`  
Tumhe **saare substrings** ki beauty ka **total sum** return karna hai.

---

# Brute Approach (Check all substrings)

## 🧠 Intuition (Hinglish)

Brute approach me hum:

- Har substring `(i...j)` generate karenge
    
- Har substring ka frequency array banayenge
    
- `maxFreq - minFreq(non-zero)` calculate karenge
    
- Sum me add kar denge
    

Yeh `O(n^3)` worst-case ho jata hai because n² substrings × O(n) counting.

---

## 📝 Dry Run

s = `"aabcb"`

- `"a"` → a=1 → 1−1=0
- `"aa"` → a=2 → 2−2=0
- `"aab"` → a=2,b=1 → 2−1=1
- `"aabc"` → 2−1=1
- `"aabcb"` → a=2,b=2,c=1 → 2−1=1  
    … similarly sare substrings → sum = **5**.
    

---

## 💻 Code (Brute)
```java
// Brute: recompute counts for each substring
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
            total += (max - min);
        }
    }
    return total;
}

```


---

# Better / Recommended Approach (Fix start, expand end)

## 🧠 Intuition (Hinglish)

Brute me hum har substring ke liye frequency baar-baar count karte hain.  
Better approach:

- Fix starting index `i`
    
- End index `j` ko expand karo
    
- Saath-saath `freq[26]` update karte jao
    
- Har step par 26 characters scan karke `maxFreq - minFreq(non-zero)` nikaal lo
    

Because alphabet fixed hai (26), complexity `O(26 * n²)` ≈ **O(n²)**.

---

## 📝 Dry Run

s = `"aabcb"`, i = 0:

- j=0 → a=1 → 1−1=0
    
- j=1 → a=2 → 2−2=0
    
- j=2 → a=2,b=1 → 2−1=1
    
- j=3 → a=2,b=1,c=1 → 2−1=1
    
- j=4 → a=2,b=2,c=1 → 2−1=1
    

Sum for i=0 = 3 → Continue similarly for i=1..n-1 → final sum = **5**

---

## 💻 Code (Better / Optimal for this problem)

```java
// Optimal for fixed alphabet: O(n^2)
public static long beautySum(String s) {
    int n = s.length();
    long total = 0;

    for (int i = 0; i < n; i++) {
        int[] freq = new int[26];
        for (int j = i; j < n; j++) {
            freq[s.charAt(j) - 'a']++;

            int max = 0;
            int min = Integer.MAX_VALUE;

            for (int f : freq) {
                if (f > 0) {
                    max = Math.max(max, f);
                    min = Math.min(min, f);
                }
            }

            total += (max - min);
        }
    }
    return total;
}

```

---

# ✅ Complexity Summary

- **Brute:** `O(n^3)`
    
- **Better / Recommended:** `O(n^2)` (because 26 is constant)
    
- **Space:** `O(1)` — 26-sized freq array
    

---

# ⚠ Edge Cases / Notes

- Empty string → result = 0
    
- Single character → har substring beauty 0
    
- All identical characters (e.g., `"aaaa"`): beauty always 0
    
- Use `long` for sum, overflow avoid karne ke liye
    

---

# Examples

- s = `"aabcb"` → **5**
    
- s = `"abc"` → 0
    
- s = `"zzzzz"` → 0