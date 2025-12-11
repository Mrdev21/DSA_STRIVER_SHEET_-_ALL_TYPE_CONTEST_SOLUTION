

**Problem (short, Hinglish):** Tumhe do strings `s` aur `t` diye gaye hain. Check karo kya yeh **anagrams** hain — matlab dono strings me **same characters** hone chahiye **same frequency** ke saath, bas order alag ho sakta hai. Agar haan → return true; nahi → false.

Examples: "listen" & "silent" → anagram; "aab" & "aba" → anagram; "aab" & "abb" → not anagram.

---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute idea: ek string ke characters ko pick karo aur doosre string me usko search karke remove karte jao. Har search O(n), total O(n^2).

---

## 📝 Dry Run

s="listen", t="silent"  
'l' search in t → found  
'i' search → found  
... sab mil jaate hain → true

---

## 💻 Code (Brute)

```java
public static boolean isAnagramBrute(String s, String t) {
    if (s.length() != t.length()) return false;
    StringBuilder temp = new StringBuilder(t);

    for (char c : s.toCharArray()) {
        int idx = temp.indexOf(String.valueOf(c));
        if (idx == -1) return false;
        temp.deleteCharAt(idx);
    }
    return true;
}
```

---

# Better Approach (Sorting)

## 🧠 Intuition (Hinglish)

Dono strings ko sort kar do. Agar sorted versions same hain → anagrams.

Easy & clean but sorting costs `O(n log n)`.

---

## 📝 Dry Run

s="listen" → eilnst  
t="silent" → eilnst  
Equal → true

---

## 💻 Code (Sorting)

```java
public static boolean isAnagramSort(String s, String t) {
    if (s.length() != t.length()) return false;
    char[] a = s.toCharArray();
    char[] b = t.toCharArray();
    Arrays.sort(a);
    Arrays.sort(b);
    return Arrays.equals(a, b);
}
```

---

# Optimal Approach (Frequency Count)

## 🧠 Intuition (Hinglish)

Count frequency of each character in `s` and subtract frequencies using characters of `t`. Agar end me sab zero ho gaye → anagram.

Ye `O(n)` time me ho jata hai with `O(1)` space (fixed alphabet like ASCII/26 letters).

---

## 📝 Dry Run

s="anagram", t="nagaram"  
CountS = {a:3, n:1, g:1, r:1, m:1}  
Subtract using t → sab zero → true

---

## 💻 Code (Optimal)

```java
public static boolean isAnagramOptimal(String s, String t) {
    if (s.length() != t.length()) return false;
    int[] freq = new int[256];

    for (char c : s.toCharArray()) freq[c]++;
    for (char c : t.toCharArray()) {
        if (--freq[c] < 0) return false;
    }
    return true;
}
```

---

# ✅ Complexity Summary

- **Brute:** `O(n^2)`
    
- **Sorting:** `O(n log n)`
    
- **Optimal (freq count):** `O(n)`, `O(1)` extra space
    

---

# ⚠ Edge Cases

- Different lengths → immediately false
    
- Case-sensitive? (depends on problem; often yes)
    
- Unicode → use HashMap instead of array
    
- Empty strings → true
    

---

# Examples

- s="anagram", t="nagaram" → true
    
- s="rat", t="car" → false
    
- s="aab", t="aba" → true
    

---

_File placement suggestion:_ `DSA/Strings/07_Check Anagram.md`