

**Problem (short, Hinglish):** Tumhe ek string diya hai. Do kaam karne hain:

1. Uske **words ko reverse** karna (word order reverse karna, characters nahi)
    
2. Ek given string **palindrome hai ya nahi** check karna.
    

Dono cheezon ke separate brute → better → optimal approaches neeche diye hain.

---

# PART 1 — Reverse Words in a Given String

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute me hum string ko split(" ") karke words list nikaal lenge, fir list ko reverse karke join kar denge. Extra space lagega.

---

## 📝 Dry Run

Input: " hello world "  
Split → ["hello","world"]  
Reverse → ["world","hello"]  
Join → "world hello"

---

## 💻 Code (Brute)

```java
public static String reverseWordsBrute(String s) {
    String[] parts = s.trim().split(" +");
    StringBuilder sb = new StringBuilder();
    for (int i = parts.length - 1; i >= 0; i--) {
        sb.append(parts[i]);
        if (i > 0) sb.append(" ");
    }
    return sb.toString();
}
```

---

# Better Approach

## 🧠 Intuition (Hinglish)

Ek hi pass me spaces ko compress karte hue words ko store karo. Fir pointers se reverse order me build karo. Better control par same complexity.

---

## 📝 Dry Run

"a good example"  
Words → [a,good,example]  
Reverse → example good a

---

## 💻 Code (Better)

```java
public static String reverseWordsBetter(String s) {
    List<String> words = new ArrayList<>();
    int i = 0, n = s.length();
    while (i < n) {
        while (i < n && s.charAt(i) == ' ') i++;
        if (i >= n) break;
        int j = i;
        while (j < n && s.charAt(j) != ' ') j++;
        words.add(s.substring(i, j));
        i = j;
    }
    Collections.reverse(words);
    return String.join(" ", words);
}
```

---

# Optimal Approach

## 🧠 Intuition (Hinglish)

Optimal usually same as better: O(n) time me parse + reverse words list.  
Space O(k) words ke liye unavoidable.

---

## 💻 Code (Optimal)

```java
public static String reverseWordsOptimal(String s) {
    String[] arr = s.trim().split(" +");
    Collections.reverse(Arrays.asList(arr));
    return String.join(" ", arr);
}
```

---

# PART 2 — Palindrome Check (String)

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute: ek new reversed string banao (character by character) aur compare kar lo. Agar same hai → palindrome.

---

## 📝 Dry Run

"racecar" → reverse = "racecar" → palindrome  
"hello" → reverse = "olleh" → not palindrome

---

## 💻 Code (Brute)

```java
public static boolean isPalindromeBrute(String s) {
    StringBuilder rev = new StringBuilder();
    for (int i = s.length()-1; i >= 0; i--) rev.append(s.charAt(i));
    return s.equals(rev.toString());
}
```

---

# Better Approach

## 🧠 Intuition (Hinglish)

Two-pointer approach: left=0, right=n-1; dono pointers move karte hue characters compare karo. Extra space nahi chahiye.

---

## 📝 Dry Run

"madam"  
L=0,M; R=4,M → match  
Next L=1,a; R=3,a → match  
L>=R → palindrome

---

## 💻 Code (Better)

```java
public static boolean isPalindromeBetter(String s) {
    int l = 0, r = s.length() - 1;
    while (l < r) {
        if (s.charAt(l) != s.charAt(r)) return false;
        l++; r--;
    }
    return true;
}
```

---

# Optimal Approach (Alphanumeric + ignore cases version)

## 🧠 Intuition (Hinglish)

Most interviewers ask for: ignore non-alphanumeric + ignore case. Use two pointers and skip unwanted chars.

---

## 💻 Code (Optimal)

```java
public static boolean isPalindromeOptimal(String s) {
    int l = 0, r = s.length() - 1;
    while (l < r) {
        char left = s.charAt(l);
        char right = s.charAt(r);

        if (!Character.isLetterOrDigit(left)) { l++; continue; }
        if (!Character.isLetterOrDigit(right)) { r--; continue; }

        if (Character.toLowerCase(left) != Character.toLowerCase(right)) return false;

        l++; r--;
    }
    return true;
}
```

---

# ⚠ Edge Cases

- Reverse words: multiple spaces, leading/trailing spaces.
    
- Palindrome: empty string → true; single char → true.
    
- Case sensitivity handled in optimal palindrome.
    

---

# Examples

**Reverse words:**

- "the sky is blue" → "blue is sky the"
    
- " hello world " → "world hello"
    

**Palindrome:**

- "racecar" → true
    
- "A man, a plan, a canal: Panama" → true
    
- "hello" → false