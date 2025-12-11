

**Problem (short, Hinglish):** Tumhe string array `strs[]` diya hai. Tumhe find karna hai longest common prefix jo **har** string me common ho. Agar koi common prefix nahi hai, return empty string `""`.

---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute: take first string as reference and check every possible prefix length from longest to shortest (or generate all prefixes) and verify for all strings — worst-case `O(n * m^2)` where `n` = number of strings, `m` = length of first string.

---

## 📝 Dry Run

strs = ["flower","flow","flight"]

- try full "flower" -> not all
    
- try "flowe" -> not all
    
- ... eventually "fl" works → return "fl"
    

---

## 💻 Code (Brute)

```java
// Brute: try prefixes of first string from longest to shortest
public static String longestCommonPrefixBrute(String[] strs) {
    if (strs == null || strs.length == 0) return "";
    String first = strs[0];
    for (int len = first.length(); len >= 0; len--) {
        String pref = first.substring(0, len);
        boolean ok = true;
        for (int i = 1; i < strs.length; i++) {
            if (!strs[i].startsWith(pref)) { ok = false; break; }
        }
        if (ok) return pref;
    }
    return "";
}
```

---

# Better Approach (Horizontal / Vertical Scanning)

## 🧠 Intuition (Hinglish)

- **Horizontal scan:** start with prefix = strs[0], compare with each next string and shrink prefix while it isn't a prefix of the string. Efficient in practice.
    
- **Vertical scan:** compare characters column-wise across strings; stop at first mismatch.
    

Both give `O(n*m)` time where `m` is length of shortest string.

---

## 📝 Dry Run (Horizontal)

strs = ["flower","flow","flight"]

- prefix="flower" vs "flow" -> shrink to "flow"
    
- prefix="flow" vs "flight" -> shrink to "fl"
    
- return "fl"
    

---

## 💻 Code (Horizontal)

```java
// Horizontal scanning: O(n * m)
public static String longestCommonPrefixHorizontal(String[] strs) {
    if (strs == null || strs.length == 0) return "";
    String prefix = strs[0];
    for (int i = 1; i < strs.length; i++) {
        while (!strs[i].startsWith(prefix)) {
            prefix = prefix.substring(0, prefix.length() - 1);
            if (prefix.isEmpty()) return "";
        }
    }
    return prefix;
}
```

---

# Optimal Approach (Divide & Conquer or Binary Search)

## 🧠 Intuition (Hinglish)

- **Divide & Conquer:** split array into halves, find LCP of each half, then LCP of results — `O(n * m)` but with good parallelization properties.
    
- **Binary Search on prefix length:** binary search on length `0..minLen` and check if all strings share prefix of that length using hashing or `startsWith` — `O(n * m * log m)` in naive string compare but practical and fast.
    

I'll provide **Divide & Conquer** implementation (clean and optimal in practice).

---

## 📝 Dry Run (Divide & Conquer)

strs = ["flower","flow","flight"]

- split -> left=[flower,flow], right=[flight]
    
- LCP(left) -> "flow" & "flower" -> "flow"
    
- LCP(right) -> "flight"
    
- LCP("flow","flight") -> "fl"
    

---

## 💻 Code (Divide & Conquer)

```java
// Divide & Conquer: O(n * m)
public static String longestCommonPrefixDivideConquer(String[] strs) {
    if (strs == null || strs.length == 0) return "";
    return lcpDC(strs, 0, strs.length - 1);
}

private static String lcpDC(String[] strs, int l, int r) {
    if (l == r) return strs[l];
    int mid = l + (r - l) / 2;
    String left = lcpDC(strs, l, mid);
    String right = lcpDC(strs, mid + 1, r);
    return commonPrefix(left, right);
}

private static String commonPrefix(String s1, String s2) {
    int min = Math.min(s1.length(), s2.length());
    for (int i = 0; i < min; i++) {
        if (s1.charAt(i) != s2.charAt(i)) return s1.substring(0, i);
    }
    return s1.substring(0, min);
}
```

---

# ✅ Complexity Summary

- **Brute:** `O(n * m^2)` worst-case
    
- **Horizontal/Vertical:** `O(n * m)` recommended
    
- **Divide & Conquer / Binary Search:** `O(n * m)` (practical and elegant)
    

---

# ⚠ Edge Cases / Notes

- Empty array or null → return `""`.
    
- Strings with empty string inside → return `""`.
    
- Consider case-sensitivity as per problem (usually case-sensitive).
    

---

# Examples

- ["flower","flow","flight"] -> "fl"
    
- ["dog","racecar","car"] -> "" (no common prefix)
    

---

_File placement suggestion:_ `DSA/Strings/04_Longest Common Prefix.md`
