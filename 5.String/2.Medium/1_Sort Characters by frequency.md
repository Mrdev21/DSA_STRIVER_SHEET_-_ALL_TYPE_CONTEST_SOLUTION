
**Problem (short, Hinglish):** Tumhe ek string `s` diya hai. Tumhe uske characters ko **frequency ke decreasing order** me sort karke output string return karna hai. Matlab jo character sabse zyada baar aaya hai, woh sabse pehle aayega. Agar multiple chars same frequency wale ho, unka order koi bhi ho sakta hai.

Example: `s = "tree"` → frequencies: t=1, r=1, e=2 → output: "eert" (ya "eetr")

---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute: har character ko count karo phir **har baar max frequency character** find karke result me add karo. Matlab har iteration me linear scan to find max.

This becomes `O(n * alphabet)` → worst-case approx `O(n^2)`.

---

## 📝 Dry Run

s = "cccaaaabb"  
Freq: c=3, a=4, b=2  
Brute repeatedly picks max:

- pick 'a' 4 times
    
- pick 'c' 3 times
    
- pick 'b' 2 times  
    Output → "aaaacccb b"
    

---

## 💻 Code (Brute)

```java
public static String frequencySortBrute(String s) {
    int[] freq = new int[256];
    for (char c : s.toCharArray()) freq[c]++;

    StringBuilder res = new StringBuilder();
    boolean done = false;
    while (!done) {
        done = true;
        int maxFreq = 0;
        char maxChar = 0;
        for (int i = 0; i < 256; i++) {
            if (freq[i] > maxFreq) {
                maxFreq = freq[i];
                maxChar = (char)i;
                done = false;
            }
        }
        for (int k = 0; k < maxFreq; k++) res.append(maxChar);
        freq[maxChar] = 0;
    }
    return res.toString();
}
```

---

# Better Approach (Sort bucket pairs)

## 🧠 Intuition (Hinglish)

- Pehle freq count karo.
    
- Fir character–frequency pairs ka list banao.
    
- List ko frequency ke decreasing order me sort kar do.
    
- Fir result string build kar lo.
    

Sorting pairs gives approx `O(n log n)`.

---

## 📝 Dry Run

s = "Aabb"  
freq: A=1, a=1, b=2  
pairs sorted → [(b,2),(A,1),(a,1)]  
result → "bbAa" (or "bb aA")

---

## 💻 Code (Better)

```java
public static String frequencySortBetter(String s) {
    int[] freq = new int[256];
    for (char c : s.toCharArray()) freq[c]++;

    List<Character> chars = new ArrayList<>();
    for (int i = 0; i < 256; i++) {
        if (freq[i] > 0) chars.add((char)i);
    }

    chars.sort((c1, c2) -> freq[c2] - freq[c1]);

    StringBuilder sb = new StringBuilder();
    for (char c : chars) {
        int f = freq[c];
        for (int i = 0; i < f; i++) sb.append(c);
    }
    return sb.toString();
}
```

---

# Optimal Approach (Bucket Sort)

## 🧠 Intuition (Hinglish)

Bucket sort trick:

1. Count freq of each char.
    
2. Make frequency buckets: `bucket[freq] = list of chars with that freq`.
    
3. Traverse buckets from highest freq → lowest.
    
4. Build output string.
    

Bucket sorting avoids full sorting → `O(n)` average.

---

## 📝 Dry Run

s = "tree"  
freq: t=1, r=1, e=2  
maxFreq=2  
buckets:

- bucket[1] → [t, r]
    
- bucket[2] → [e]  
    Output → "ee" + "tr" → "eert"
    

---

## 💻 Code (Optimal)

```java
public static String frequencySortOptimal(String s) {
    int[] freq = new int[256];
    int maxFreq = 0;

    for (char c : s.toCharArray()) {
        freq[c]++;
        maxFreq = Math.max(maxFreq, freq[c]);
    }

    List<Character>[] buckets = new ArrayList[maxFreq + 1];
    for (int i = 0; i <= maxFreq; i++) buckets[i] = new ArrayList<>();

    for (int i = 0; i < 256; i++) {
        if (freq[i] > 0) buckets[freq[i]].add((char)i);
    }

    StringBuilder sb = new StringBuilder();
    for (int f = maxFreq; f >= 1; f--) {
        for (char c : buckets[f]) {
            for (int i = 0; i < f; i++) sb.append(c);
        }
    }
    return sb.toString();
}
```

---

# ✅ Complexity Summary

- **Brute:** `O(n^2)` time
    
- **Better:** `O(n log n)`
    
- **Optimal (Bucket Sort):** `O(n)` time, `O(1)` space (fixed 256 alphabet)
    

---

# ⚠ Edge Cases

- Empty string → return ""
    
- All chars same → return original string
    
- Upper/lowercase treated separately unless specified
    
- For Unicode, array size adjust or use HashMap
    

---

# Examples

- s="tree" → "eert" / "eetr"
    
- s="cccaaa" → "cccaaa" or "aaaccc"
    
- s="Aabb" → "bbAa"
    

---

_File placement suggestion:_ `DSA/Strings/01_Sort Characters By Frequency.md`