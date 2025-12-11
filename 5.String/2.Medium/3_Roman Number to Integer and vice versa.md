

**Problem (short, Hinglish):** Tumhe Roman numerals aur integers ke beech conversion karna hai:

1. **Roman → Integer**: Roman string ko parse karke uska integer value return karo.
    
2. **Integer → Roman**: Given integer (1–3999), uska Roman numeral banaakar return karo.
    

---

# PART 1 — Roman to Integer

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute me har symbol ka value dekhte jao aur agar aage wala symbol bada hai, to subtract karna hai; warna add karna hai. Ye typical rule hai Roman numeral ka.

Brute & optimal almost same hi hain is problem me.

---

## 📝 Dry Run

"MCMXCIV" → 1994  
M(1000) → add → total=1000  
C(100) before M(1000) → subtract 100 → total=900  
X(10) before C(100) → subtract 10 → total=890  
I(1) before V(5) → subtract 1 → total=894  
... finally → 1994

---

## 💻 Code (Roman → Integer)

```java
public static int romanToInt(String s) {
    Map<Character, Integer> map = new HashMap<>();
    map.put('I', 1);
    map.put('V', 5);
    map.put('X', 10);
    map.put('L', 50);
    map.put('C', 100);
    map.put('D', 500);
    map.put('M', 1000);

    int total = 0;
    for (int i = 0; i < s.length(); i++) {
        int val = map.get(s.charAt(i));
        if (i + 1 < s.length() && map.get(s.charAt(i + 1)) > val) {
            total -= val;
        } else {
            total += val;
        }
    }
    return total;
}
```

---

# Optimal Approach (Same logic)

## 🧠 Intuition (Hinglish)

Optimal = same subtractive rule. O(n) time and O(1) space.

No further optimization needed.

---

# PART 2 — Integer to Roman

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute me repeatedly subtract karo:

- 1000 se jitna subtract ho → 'M' add karo
    
- phir 900, 500, 400 ... etc  
    Values ko descending list me rakho aur greedy approach use karo.
    

---

## 📝 Dry Run

1994 →

- 1000 → 'M'
    
- 900 → 'CM'
    
- 90 → 'XC'
    
- 4 → 'IV'  
    Final → "MCMXCIV"
    

---

## 💻 Code (Integer → Roman)

```java
public static String intToRoman(int num) {
    int[] values = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
    String[] symbols = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < values.length; i++) {
        while (num >= values[i]) {
            num -= values[i];
            sb.append(symbols[i]);
        }
    }
    return sb.toString();
}
```

---

# Optimal Approach

## 🧠 Intuition (Hinglish)

Greedy method hi optimal hai. Pre-defined values & symbols list se largest possible subtract karte raho. O(1) time because Roman conversion fixed size (max 3999).

---

# ✅ Complexity Summary

- Roman → Integer: **O(n)** time, **O(1)** space
    
- Integer → Roman: **O(1)** time (fixed set of conversions), **O(1)** space
    

---

# ⚠ Edge Cases

- Roman input must be valid (problem usually guarantees)
    
- Only numbers 1–3999 allowed for integer → roman
    
- Lowercase/inconsistent Roman letters handle as uppercase or restrict
    

---

# Examples

- Roman → Int: "LVIII" → 58, "MCMXCIV" → 1994
    
- Int → Roman: 3749 → "MMMDCCXLIX", 58 → "LVIII"
    

---

_File placement suggestion:_ `DSA/Strings/03_Roman to Int and Int to Roman.md`