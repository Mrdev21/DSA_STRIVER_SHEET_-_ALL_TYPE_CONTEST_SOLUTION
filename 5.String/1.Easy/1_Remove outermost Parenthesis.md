

**Problem (short, Hinglish):** Tumhe ek valid parentheses string `s` diya hai jo multiple **primitive** parentheses strings se milkar bana hota hai. Tumhe har primitive string ke **outermost parentheses remove** karke final string return karna hai.

Primitive string = a parenthesis string jo aur further split nahi ho sakta.  
Example: "(()())" primitive nahi, par "()" and "(())" ke andar primitives bante hain.

Goal: outer layer hatani hai, andar ka structure same rakho.

---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute me pehle pure string ko primitive parts me tod do:

- Har baar jab balance = 0 hota hai, ek primitive complete hota hai.  
    Fir har primitive ke first '(' aur last ')' remove kar do aur result me add kar do.
    

Ye approach easy hai but extra space lagta hai.

---

## 📝 Dry Run

s = "(()())(())"  
Balance tracking:

- "(()())" → primitive 1
    
- "(())" → primitive 2  
    Remove outer layer:
    
- primitive1: (()()) → {} inside "()()" → "()()"
    
- primitive2: (()) → remove outer → "()"  
    Final: "()()()"
    

---

## 💻 Code (Brute)

```java
public static String removeOuterBracketsBrute(String s) {
    StringBuilder ans = new StringBuilder();
    int bal = 0;
    StringBuilder temp = new StringBuilder();

    for (char c : s.toCharArray()) {
        if (c == '(') {
            if (bal > 0) temp.append(c);
            bal++;
        } else {
            bal--;
            if (bal > 0) temp.append(c);
            if (bal == 0) {
                ans.append(temp.toString());
                temp.setLength(0);
            }
        }
    }
    return ans.toString();
}
```

---

# Better Approach

## 🧠 Intuition (Hinglish)

Better tareeka ye hai ki string ko primitive me split karne ki zaroorat nahi hai.  
Simply balance track karo:

- '(' milne par agar balance > 0 ho tabhi result me add karo.
    
- ')' milne par agar closing ke baad balance > 0 rahe tab add karo.
    

Isse outermost parentheses skip ho jaate hain.

---

## 📝 Dry Run

s = "(()())(())"  
Steps:

- '(': bal=1 → skip (outermost)
    
- '(': bal=2 → add
    
- ')': bal=1 → add
    
- ')': bal=0 → skip
    
- '(' ... and so on
    

---

## 💻 Code (Better)

```java
public static String removeOuterParenthesesBetter(String s) {
    StringBuilder sb = new StringBuilder();
    int bal = 0;

    for (char c : s.toCharArray()) {
        if (c == '(') {
            if (bal > 0) sb.append(c);
            bal++;
        } else {
            bal--;
            if (bal > 0) sb.append(c);
        }
    }
    return sb.toString();
}
```

---

# Optimal Approach (Same as Better) — O(n), O(1) extra space

## 🧠 Intuition (Hinglish)

Is problem ka optimal = better approach hi hai.  
Balanced parentheses traversal with simple rules ensures single pass `O(n)`.

No further optimization possible.

---

## 💻 Code (Optimal)

```java
public static String removeOuterParenthesesOptimal(String s) {
    StringBuilder res = new StringBuilder();
    int bal = 0;

    for (char c : s.toCharArray()) {
        if (c == '(') {
            if (bal > 0) res.append(c);
            bal++;
        } else {
            bal--;
            if (bal > 0) res.append(c);
        }
    }
    return res.toString();
}
```

---

# ✅ Complexity Summary

- **Time:** `O(n)`
    
- **Space:** `O(n)` (or `O(1)` auxiliary)
    

---

# ⚠ Edge Cases

- Already primitive: "()" → output = ""
    
- Deep nested: "((()))" → output = "(())"
    
- Multiple primitives: "()()" → output = ""
    

---

# Examples

- Input: "(()())(())" → Output: "()()()"
    
- Input: "(()())" → Output: "()()"
    
- Input: "()" → Output: ""
