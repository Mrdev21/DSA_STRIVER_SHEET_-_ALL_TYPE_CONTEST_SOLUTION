
**Problem (short, Hinglish):** Tumhe ek valid parentheses string `s` (sirf '(' aur ')') diya hai — find karo uska **maximum nesting depth** (ya maximum parenthesis nesting level). Yani deepest level jahan tak parentheses nested hain. Return an integer depth.

Example: `s = "(1+(2*3)+((8)/4))+1"` → maximum nesting = 3 (because of `((8)/4)`).

---

# Brute Approach (Stack)

## 🧠 Intuition (Hinglish)

Brute idea: use a stack. Jab '(' aata hai push karo, jab ')' aata hai pop karo. Track stack size after each push — maximum stack size = nesting depth.

Works fine and is straightforward, but uses `O(n)` extra space for the stack.

---

## 📝 Dry Run

s = "( ( ) ( ( ) ) )" (spaces for clarity) → equivalently "(()(()))"

- read '(': push -> size=1 -> max=1
    
- read '(': push -> size=2 -> max=2
    
- read ')': pop -> size=1
    
- read '(': push -> size=2
    
- read '(': push -> size=3 -> max=3
    
- read ')': pop -> size=2
    
- read ')': pop -> size=1
    
- read ')': pop -> size=0  
    Answer = 3
    

---

## 💻 Code (Java)

```java
// Stack approach: O(n) time, O(n) space
public static int maxDepthStack(String s) {
    Deque<Character> stack = new ArrayDeque<>();
    int maxDepth = 0;
    for (char c : s.toCharArray()) {
        if (c == '(') {
            stack.push(c);
            maxDepth = Math.max(maxDepth, stack.size());
        } else if (c == ')') {
            if (!stack.isEmpty()) stack.pop();
        }
        // ignore other characters
    }
    return maxDepth;
}
```

---

# Better Approach (Counter / Single-pass)

## 🧠 Intuition (Hinglish)

Optimal simple trick: stack is not needed. Use an integer `depth` counter:

- `depth++` on '(' and update `maxDepth = max(maxDepth, depth)`
    
- `depth--` on ')'
    

Single pass `O(n)` time and `O(1)` extra space.

---

## 📝 Dry Run

s = "(1+(2*3)+((8)/4))+1"  
Traverse chars:

- '(': depth=1 -> max=1
    
- '1' ignore
    
- '+' ignore
    
- '(': depth=2 -> max=2
    
- ...
    
- '(': depth=3 -> max=3
    
- ')' depth=2
    
- ... end -> answer 3
    

---

## 💻 Code (Java)

```java
// Counter approach: O(n) time, O(1) space
public static int maxDepth(String s) {
    int depth = 0, maxDepth = 0;
    for (char c : s.toCharArray()) {
        if (c == '(') {
            depth++;
            if (depth > maxDepth) maxDepth = depth;
        } else if (c == ')') {
            depth--; // assume valid string so depth never negative
        }
    }
    return maxDepth;
}
```

---

# ✅ Complexity Summary

- Time: `O(n)` (single pass)
    
- Space: `O(1)` for counter approach; `O(n)` if using stack
    

---

# ⚠ Edge Cases / Notes

- If input may be invalid (unbalanced), validate by ensuring `depth` never becomes negative and final `depth` is zero. Return -1 or throw exception on invalid input if required.
    
- Characters other than '(' and ')' should be ignored (like digits, operators, spaces).
    
- For very long strings, depth fits in `int` normally (depth ≤ length). If needed use `long` but not typical.
    

---

# Examples

- s = "(())" → 2
    
- s = "()()" → 1
    
- s = "" → 0
    
- s = "(((x)))" → 3
    

---

_File placement suggestion:_ `DSA/Strings/02_Maximum Nesting Depth of Parentheses.md`