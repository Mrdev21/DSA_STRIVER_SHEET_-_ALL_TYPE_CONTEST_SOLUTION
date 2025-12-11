

**Problem (short, Hinglish):** Tumhe ek string `s` diya hai — find karo uska **longest palindromic substring**. (Requirement: solutions **without DP** — use expand-around-center or Manacher's algorithm.)

---

# Brute Approach (Check all substrings)

## 🧠 Intuition (Hinglish)

Brute me hum har possible substring generate karenge (start i, end j) aur check karenge kya woh palindrome hai (reverse ya two-pointer check). Yeh `O(n^3)` worst-case (n^2 substrings × O(n) check) — only for conceptual completeness.

---

## 📝 Dry Run

s = "babad"  
All substrings: "b","a","b","a","d","ba","ab","ba","ad",... Palindromic ones include "b","a","bab","aba". Longest length = 3 → could return "bab" or "aba".

---

## 💻 Code (Brute)

```java
// Brute: check all substrings
public static String longestPalindromeBrute(String s) {
    int n = s.length();
    String best = "";
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            if (isPal(s, i, j) && (j - i + 1) > best.length()) {
                best = s.substring(i, j + 1);
            }
        }
    }
    return best;
}

private static boolean isPal(String s, int l, int r) {
    while (l < r) {
        if (s.charAt(l++) != s.charAt(r--)) return false;
    }
    return true;
}
```

---

# Better Approach (Expand Around Center) — Recommended if n ≤ 10^4

## 🧠 Intuition (Hinglish)

Har palindrome center ko consider karo (center can be a character for odd length, or between two chars for even). For each center expand left & right while chars match. Track longest found. Yeh `O(n^2)` worst-case (e.g., all same characters) lekin simple aur interview-friendly.

---

## 📝 Dry Run

s = "cbbd"

- center at 'c' → expand -> "c"
    
- center between 'c' and 'b' -> none
    
- center at first 'b' -> expand to "bb" (length 2) → best so far
    
- center at second 'b' -> similar
    
- result = "bb"
    

---

## 💻 Code (Java)

```java
// Expand around center: O(n^2) time, O(1) extra space
public static String longestPalindromeExpand(String s) {
    if (s == null || s.length() < 1) return "";
    int start = 0, end = 0;
    for (int i = 0; i < s.length(); i++) {
        int len1 = expandFrom(s, i, i);       // odd
        int len2 = expandFrom(s, i, i + 1);   // even
        int len = Math.max(len1, len2);
        if (len > end - start + 1) {
            start = i - (len - 1) / 2;
            end = i + len / 2;
        }
    }
    return s.substring(start, end + 1);
}

private static int expandFrom(String s, int left, int right) {
    while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
        left--; right++;
    }
    return right - left - 1; // length of palindrome
}
```

---

# Optimal Approach (Manacher's Algorithm) — O(n)

## 🧠 Intuition (Hinglish)

Manacher transforms string to handle even/odd palindromes uniformly (insert separators like `#`), then maintains a `center` and `right` boundary of the rightmost palindrome found. For each position, it uses previously computed palindrome lengths (mirror property) to skip unnecessary comparisons, and then expands if needed. Overall complexity `O(n)`.

Manacher is more complex but gives linear time — useful for very long strings.

---

## 📝 Dry Run (sketch)

s = "abacdfgdcaba"

- Transformed: ^#a#b#a#c#d#f#g#d#c#a#b#a#$ (sentinels)
    
- Iterate and compute p[i] array of palindrome radii using mirror and expand rules. Track max radius and center → compute original substring.
    

---

## 💻 Code (Java)

```java
// Manacher's algorithm: O(n) time
public static String longestPalindromeManacher(String s) {
    if (s == null || s.length() == 0) return "";
    // Transform s to t with separators
    char[] t = preprocess(s);
    int n = t.length;
    int[] p = new int[n];
    int center = 0, right = 0;
    int maxLen = 0, centerIndex = 0;

    for (int i = 1; i < n - 1; i++) {
        int mirror = 2 * center - i;
        if (i < right) p[i] = Math.min(right - i, p[mirror]);
        else p[i] = 0;

        // expand around i
        while (t[i + 1 + p[i]] == t[i - 1 - p[i]]) p[i]++;

        // update center/right
        if (i + p[i] > right) {
            center = i;
            right = i + p[i];
        }

        if (p[i] > maxLen) {
            maxLen = p[i];
            centerIndex = i;
        }
    }

    int start = (centerIndex - maxLen) / 2; // map back to original string indices
    return s.substring(start, start + maxLen);
}

private static char[] preprocess(String s) {
    // add sentinels to avoid bounds check
    char[] t = new char[s.length() * 2 + 3];
    t[0] = '^';
    int idx = 1;
    for (int i = 0; i < s.length(); i++) {
        t[idx++] = '#';
        t[idx++] = s.charAt(i);
    }
    t[idx++] = '#';
    t[idx++] = '$';
    return t;
}
```

---

# ✅ Complexity Summary (without DP)

- **Brute:** `O(n^3)` time, `O(1)` extra space
    
- **Expand Around Center:** `O(n^2)` time, `O(1)` extra space (practical & simple)
    
- **Manacher:** `O(n)` time, `O(n)` extra space for transformed array and p[] (best for very long strings)
    

---

# ⚠ Edge Cases / Notes

- If multiple palindromes tie in length, any one is acceptable (problem-dependent).
    
- Empty string → return `""`
    
- Manacher requires careful index mapping when extracting substring.
    

---

# Examples

- s = "babad" → "bab" or "aba"
    
- s = "cbbd" → "bb"
    
- s = "a" → "a"
    

---

_File placement suggestion:_ `DSA/Strings/06_Longest Palindromic Substring.md`