
**Problem (short, Hinglish):** Tumhe ek numeric string `s` di hui hai (digits only). Tumhe us string ka **largest odd-numbered substring** return karna hai — where "largest" means the longest possible (not numerically largest): practically problem commonly defined as: remove the minimum possible suffix so that the remaining string represents an odd number (i.e., its last digit is odd). Agar aisa koi odd-numbered substring na mile to empty string return karo.

Example interpretation (common): Return the **longest prefix** of `s` that is an odd number (by trimming trailing digits until last digit is odd).

---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute: generate **all substrings** of `s`, filter those that have odd last digit, and pick the longest (or numerically largest if asked). Generating all substrings is `O(n^2)` and comparing (or parsing) makes it heavy.

This is only for conceptual correctness demonstration.

---

## 📝 Dry Run

s = "55243"  
All substrings that end with an odd digit: "5","55","552","5524" (no - these end with even), actually substrings ending with odd digits: last char '3' -> many substrings ending at index 4: "3","43","243","5243","55243" → longest is "55243" (already odd), answer = "55243". If last digit even, we'd look for earlier odd.

---

## 💻 Code (Brute)

```java
// Brute: check all substrings O(n^2)
public static String largestOddBrute(String s) {
    int n = s.length();
    String best = "";
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            if (isOdd(s.charAt(j))) {
                String sub = s.substring(i, j + 1);
                if (sub.length() > best.length()) best = sub;
            }
        }
    }
    return best;
}

private static boolean isOdd(char c) {
    int d = c - '0';
    return (d & 1) == 1;
}
```

---

# Better / Optimal Approach (Single pass from right)

## 🧠 Intuition (Hinglish)

Simplest trick: longest prefix that is odd equals the whole string trimmed at the **rightmost odd digit** — find the last odd digit index `i` scanning from right; if found return `s.substring(0, i+1)`, else return `""`.

Reason: any odd-numbered substring that is longer than that would have to include trailing characters beyond the last odd digit (which are even), making it even. Any substring shorter than that is not longer than prefix upto last odd digit.

Time `O(n)` and `O(1)` extra space.

---

## 📝 Dry Run

s = "4206"

- scan from right: digits 6 (even), 0 (even), 2 (even), 4 (even) → no odd found → return ""
    

s = "3542"

- scan from right: 2 even, 4 even, 5 odd at idx=1 → return s.substring(0,2) = "35" (which ends with 5 odd). That's the longest prefix that is odd.
    

s = "55243" → last odd at idx=4 -> return full string "55243".

---

## 💻 Code (Java)

```java
// Optimal: O(n) scan from right
public static String largestOddNumber(String s) {
    for (int i = s.length() - 1; i >= 0; i--) {
        char c = s.charAt(i);
        if ((c - '0') % 2 == 1) return s.substring(0, i + 1);
    }
    return "";
}
```

---

# ✅ Complexity Summary

- Brute: `O(n^2)` time, `O(n)` space for substrings (or `O(1)` if comparing lengths only)
    
- Optimal: `O(n)` time, `O(1)` extra space
    

---

# ⚠ Edge Cases / Notes

- Leading zeros allowed (treated as part of substring). If problem requires trimming leading zeros, handle separately.
    
- If the entire string is odd (last digit odd) → return original string.
    
- If no odd digit → return `""`.
    
- If problem variant asks for **numerically largest odd substring**, logic differs (need to compare lengths then lexicographically), but common LC problem wants longest prefix ending in odd digit.
    

---

# Examples

- s = "52" → "5" (last odd at index 0)
    
- s = "4206" → ""
    
- s = "35427" → "35427"
    

---

_File placement suggestion:_ `DSA/Strings/03_Largest odd number in a string.md`