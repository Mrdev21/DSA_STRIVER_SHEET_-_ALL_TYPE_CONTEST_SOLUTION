
**Problem (short, Hinglish):** Do strings `s` aur `t` diye hain — check karo kya woh **isomorphic** hain. Matlab: har character in `s` can be replaced with some character to get `t`, preserving order, and mapping must be **one-to-one** (bijective) between characters of `s` and `t`. Ek character ko do alag characters me map nahi kar sakte aur do different chars same char pe map nahi hone chahiye.

Example: `egg` and `add` → isomorphic (e->a, g->d). `foo` and `bar` → not isomorphic (o maps to two chars).

---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute idea: try all possible mappings from characters of `s` to characters of `t` and check if a consistent bijection exists. Ye exponential ho jayega (huge) and impractical — sirf conceptual.

---

## 📝 Dry Run

s = "egg", t = "add"

- try mapping e->a, g->d → consistent → true  
    s = "foo", t = "bar"
    
- f->b, o->a for first o then second o should map to a but sees r -> mismatch → false
    

---

## 💻 Code (Brute - conceptual, not provided)

(Brute mapping enumeration omitted because impractical.)

---

# Better Approach (Two maps)

## 🧠 Intuition (Hinglish)

Simple and common approach: maintain two hash maps (or arrays for ASCII) — `mapS` from chars of `s` to chars of `t`, and `mapT` from chars of `t` to chars of `s`. Iterate through characters:

- If `s[i]` in `mapS` then it must map to `t[i]` else fail.
    
- If `t[i]` in `mapT` then it must map to `s[i]` else fail.
    
- Else assign both mappings.
    

This ensures bijection (one-to-one).

Time `O(n)`, space `O(1)` if charset fixed (e.g., ASCII/Unicode small), else `O(min(n,alphabet))`.

---

## 📝 Dry Run

s="paper", t="title"

- p->t, a->i, p already maps to t OK, e->l, r->e → true
    

---

## 💻 Code (Java)

```java
// Two-hash approach: O(n) time
public static boolean isIsomorphicTwoMaps(String s, String t) {
    if (s.length() != t.length()) return false;
    int n = s.length();
    Map<Character, Character> mapS = new HashMap<>();
    Map<Character, Character> mapT = new HashMap<>();

    for (int i = 0; i < n; i++) {
        char cs = s.charAt(i);
        char ct = t.charAt(i);
        if (mapS.containsKey(cs)) {
            if (mapS.get(cs) != ct) return false;
        } else {
            mapS.put(cs, ct);
        }
        if (mapT.containsKey(ct)) {
            if (mapT.get(ct) != cs) return false;
        } else {
            mapT.put(ct, cs);
        }
    }
    return true;
}
```

---

# Optimal Approach (Single pass with arrays)

## 🧠 Intuition (Hinglish)

For small fixed alphabets (like ASCII), we can use two fixed-size arrays (size 256 or 128) initialized to -1 to store mappings, or store last seen positions. Another elegant trick: store last seen index positions for characters of `s` and `t` in two arrays and ensure they match at each step.

Example trick: `lastS[cs] == lastT[ct]` at every position (where last arrays store the last index+1 when the char appeared). Use `i+1` to differentiate from default 0.

This is `O(n)` time and `O(1)` space (arrays of fixed size).

---

## 📝 Dry Run

s="egg", t="add"

- i=0: lastS['e']=0, lastT['a']=0 → set both to 1
    
- i=1: lastS['g']=0, lastT['d']=0 → set both to 2
    
- i=2: lastS['g']=2, lastT['d']=2 → equal → continue → true
    

---

## 💻 Code (Java)

```java
// Optimal: using arrays of last seen positions
public static boolean isIsomorphicOptimal(String s, String t) {
    if (s.length() != t.length()) return false;
    int[] lastS = new int[256];
    int[] lastT = new int[256];
    int n = s.length();
    for (int i = 0; i < n; i++) {
        int cs = s.charAt(i);
        int ct = t.charAt(i);
        if (lastS[cs] != lastT[ct]) return false;
        lastS[cs] = i + 1;
        lastT[ct] = i + 1;
    }
    return true;
}
```

---

# ✅ Complexity Summary

- Time: `O(n)`
    
- Space: `O(1)` (fixed alphabet arrays) or `O(min(n, alphabet))` for hash maps
    

---

# ⚠ Edge Cases / Notes

- Strings of different lengths → false
    
- Unicode vs ASCII: choose arrays size accordingly or use HashMap for general Unicode
    
- Empty strings → true
    

---

# Examples

- s="egg", t="add" → true
    
- s="foo", t="bar" → false
    
- s="paper", t="title" → true
    

---

_File placement suggestion:_ `DSA/Strings/05_Isomorphic String.md`