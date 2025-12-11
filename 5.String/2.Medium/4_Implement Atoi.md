
**Problem (short, Hinglish):** Tumhe ek string `s` diya hai jisme number ho sakta hai, sign ho sakta hai, spaces ho sakti hain. Tumhe **atoi (string to integer)** implement karna hai — bilkul C/C++ ke `atoi()` jaisa. Matlab:

- Leading spaces ignore karo.
    
- Optional '+' ya '-' sign handle karo.
    
- Next continuous digits ko number me convert karo.
    
- Non-digit aate hi stop.
    
- Overflow/underflow hone par clamp to INT_MIN/INT_MAX.
    
- Agar valid number na ho → return 0.
    

---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute idea: string ko clean karo (spaces hatado), fir har char ko digit check karke result = result * 10 + digit format me banao. Overflow ka check baad me karna padta hai — brute me usually try-catch (parseInt) use karke bhi log kar sakte ho, but interview me allowed nahi hota.

Isliye brute me manual multiplication karna par hi padta hai.

---

## 📝 Dry Run

s = " -42abc"

- trim → "-42abc"
    
- sign = -1
    
- digits read: 4 → num=4; 2 → num=42
    
- 'a' non-digit → stop
    
- answer = -42
    

---

## 💻 Code (Brute)

```java
public static int myAtoiBrute(String s) {
    s = s.trim();
    if (s.isEmpty()) return 0;

    int sign = 1;
    int i = 0;
    long num = 0;

    if (s.charAt(0) == '+' || s.charAt(0) == '-') {
        sign = (s.charAt(0) == '-') ? -1 : 1;
        i++;
    }

    for (; i < s.length(); i++) {
        char c = s.charAt(i);
        if (!Character.isDigit(c)) break;
        num = num * 10 + (c - '0');
    }

    num *= sign;

    if (num > Integer.MAX_VALUE) return Integer.MAX_VALUE;
    if (num < Integer.MIN_VALUE) return Integer.MIN_VALUE;

    return (int) num;
}
```

---

# Better Approach (Manual Overflow Check)

## 🧠 Intuition (Hinglish)

Overflow tab hota hai jab:

- `result > INT_MAX / 10` OR
    
- `result == INT_MAX / 10 && next_digit > INT_MAX % 10`
    

Same for negative.

Is logic ko apply karke overflow aane se pehle hi clamp return kar dena.

---

## 📝 Dry Run

INT_MAX = 2147483647  
Suppose result=214748364, next digit=9

- result = INT_MAX/10 (214748364)
    
- next digit(9) > lastDigit(7) → overflow → clamp INT_MAX
    

---

## 💻 Code (Better)

```java
public static int myAtoiBetter(String s) {
    int i = 0, n = s.length();

    // skip spaces
    while (i < n && s.charAt(i) == ' ') i++;
    if (i == n) return 0;

    int sign = 1;
    if (s.charAt(i) == '+' || s.charAt(i) == '-') {
        sign = (s.charAt(i) == '-') ? -1 : 1;
        i++;
    }

    int result = 0;
    while (i < n && Character.isDigit(s.charAt(i))) {
        int d = s.charAt(i) - '0';

        // check overflow
        if (result > Integer.MAX_VALUE / 10 || (result == Integer.MAX_VALUE / 10 && d > 7)) {
            return (sign == 1) ? Integer.MAX_VALUE : Integer.MIN_VALUE;
        }

        result = result * 10 + d;
        i++;
    }

    return result * sign;
}
```

---

# Optimal Approach (Same as Better — O(n), O(1))

## 🧠 Intuition (Hinglish)

Atoi ka optimal yehi hai: single pass, manual overflow detection. Time `O(n)` and space `O(1)`.

No faster method exists.

---

# ✅ Complexity Summary

- Time: `O(n)`
    
- Space: `O(1)`
    

---

# ⚠ Edge Cases

- " " → 0
    
- " +00123" → 123
    
- "-91283472332" → INT_MIN
    
- "+-12" → invalid after sign → 0
    
- "words123" → 0
    
- "123words" → 123
    

---

# Examples

- s = "42" → 42
    
- s = " -42" → -42
    
- s = "4193 with words" → 4193
    
- s = "21474836460" → INT_MAX
    
- s = "-91283472332" → INT_MIN
    

---

_File placement suggestion:_ `DSA/Strings/04_Implement Atoi.md`