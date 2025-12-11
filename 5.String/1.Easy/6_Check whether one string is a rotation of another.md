
**Problem (short, Hinglish):** Tumhe do strings `s` aur `t` diye gaye hain — check karo kya `t` kisi rotation (circular shift) se `s` ban sakta hai. Matlab agar `s = "abcde"`, to `t = "cdeab"` ek rotation hai. Return true/false.

---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute: sab possible rotations of `t` (ya `s`) generate karo aur check karo agar koi rotation `s` ke barabar ho. Har rotation me string build karna O(n), aur rotations n hain → `O(n^2)` time.

---

## 📝 Dry Run

s = "waterbottle", t = "erbottlewat"  
Generate rotations of t:

- "erbottlewat" -> equals s? yes -> return true
    

---

## 💻 Code (Brute)

```java
// Brute: generate all rotations of t and compare
public static boolean isRotationBrute(String s, String t) {
    if (s.length() != t.length()) return false;
    int n = t.length();
    String cur = t;
    for (int i = 0; i < n; i++) {
        if (cur.equals(s)) return true;
        cur = cur.substring(1) + cur.charAt(0); // rotate left by 1
    }
    return false;
}
```

---

# Better Approach (Concatenate & Search)

## 🧠 Intuition (Hinglish)

Classic trick: agar `t` rotation of `s` hai, to `t` is a substring of `s+s`. So check `s.length==t.length` and then `(s+s).contains(t)`.

Time depends on substring search implementation — naive is `O(n^2)` but using efficient search (KMP) gives `O(n)`.

---

## 📝 Dry Run

s = "waterbottle", t = "erbottlewat"  
s+s = "waterbottlewaterbottle" -> contains("erbottlewat")? yes -> return true

---

## 💻 Code (Better - using built-in contains)

```java
// Better: s+s contains t
public static boolean isRotationConcat(String s, String t) {
    if (s.length() != t.length()) return false;
    String doubled = s + s;
    return doubled.contains(t);
}
```

---

# Optimal Approach (KMP for substring search)

## 🧠 Intuition (Hinglish)

To guarantee linear time `O(n)` worst-case, use KMP to search `t` inside `s+s`. Build LPS on `t` and run KMP on `s+s` (or run KMP concatenating pattern and text appropriately).

This gives `O(n)` time and `O(n)` space for LPS.

---

## 📝 Dry Run

s = "abcde", t = "cdeab"  
s+s = "abcdeabcde" → KMP will find match starting at index 2 → true

---

## 💻 Code (Optimal - KMP)

```java
// KMP substring search: O(n) time
public static boolean isRotationKMP(String s, String t) {
    if (s.length() != t.length()) return false;
    String text = s + s;
    return kmpSearch(text, t);
}

private static boolean kmpSearch(String text, String pat) {
    int[] lps = buildLPS(pat);
    int i = 0, j = 0; // i->text, j->pat
    while (i < text.length()) {
        if (text.charAt(i) == pat.charAt(j)) {
            i++; j++;
            if (j == pat.length()) return true;
        } else {
            if (j != 0) j = lps[j - 1];
            else i++;
        }
    }
    return false;
}

private static int[] buildLPS(String pat) {
    int n = pat.length();
    int[] lps = new int[n];
    int len = 0; // length of previous longest prefix suffix
    int i = 1;
    while (i < n) {
        if (pat.charAt(i) == pat.charAt(len)) {
            len++; lps[i] = len; i++;
        } else {
            if (len != 0) len = lps[len - 1];
            else { lps[i] = 0; i++; }
        }
    }
    return lps;
}
```

---

# ✅ Complexity Summary

- **Brute:** `O(n^2)` time, `O(1)` extra space
    
- **Better (concat + contains):** average `O(n)` if `contains` optimized, worst-case `O(n^2)` with naive search
    
- **Optimal (KMP):** `O(n)` time, `O(n)` extra space for LPS
    

---

# ⚠ Edge Cases / Notes

- Check lengths equal first (else false)
    
- Empty strings: treat `""` rotated of `""` as true
    
- Use KMP if worst-case linear time required (very long inputs)
    

---

# Examples

- s="waterbottle", t="erbottlewat" → true
    
- s="abcde", t="abced" → false
    

---

_File placement suggestion:_ `DSA/Strings/06_Check whether one string is a rotation of another.md`