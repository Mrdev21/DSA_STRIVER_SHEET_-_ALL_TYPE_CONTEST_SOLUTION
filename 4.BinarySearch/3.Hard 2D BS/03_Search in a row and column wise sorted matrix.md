
**Problem (short, Hinglish):** Tumhe ek 2D matrix di hui hai jisme **har row left→right sorted** hai aur **har column top→down sorted** hai. Tumhe ek `target` value diya hai — check karo kya woh matrix me present hai ya nahi. Return `true/false`.

(Interview me clarify karna: yeh **row & column sorted** variant hai — not the flattened-sorted one.)

---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute me simply pura matrix scan kar lo row-by-row aur har cell check karo. Simple and always correct.

## 📝 Dry Run

mat = [[1,4,7,11],[2,5,8,12],[3,6,9,16]], target = 9

- scan until (2,2)=9 → found
    

## 💻 Code (Java)

```java
// Brute: O(n*m) scan
public static boolean searchMatrixBrute(int[][] mat, int target) {
    int n = mat.length;
    if (n == 0) return false;
    int m = mat[0].length;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (mat[i][j] == target) return true;
        }
    }
    return false;
}
```

---

# Better Approach (Binary Search per Row)

## 🧠 Intuition (Hinglish)

Har row sorted hai — toh har row me binary search kar sakte ho. Total time `O(n log m)`.

Useful jab `m` bohot bada hai aur `n` chhota.

## 📝 Dry Run

mat row1=[1,4,7,11] → binary search for 9 → not found  
row2=[2,5,8,12] → not found  
row3=[3,6,9,16] → binary search finds 9 → true

## 💻 Code (Java)

```java
// Binary search each row: O(n log m)
public static boolean searchMatrixPerRowBinary(int[][] mat, int target) {
    int n = mat.length;
    if (n == 0) return false;
    int m = mat[0].length;
    for (int i = 0; i < n; i++) {
        int low = 0, high = m - 1;
        while (low <= high) {
            int mid = low + (high - low) / 2;
            if (mat[i][mid] == target) return true;
            else if (mat[i][mid] < target) low = mid + 1;
            else high = mid - 1;
        }
    }
    return false;
}
```

---

# Optimal Approach (Staircase Search from Top-Right)

## 🧠 Intuition (Hinglish)

Best trick: **start at top-right corner** `(0, m-1)`.

- Agar current == target → found
    
- Agar current > target → `j--` (move left) because left values smaller
    
- Agar current < target → `i++` (move down) because lower values larger
    

Yeh walk monotonic hai aur worst-case `O(n + m)` steps.

## 📝 Dry Run

mat = [[1,4,7,11],[2,5,8,12],[3,6,9,16]], target = 9

- start (0,3)=11 >9 → left to (0,2)=7
    
- 7 <9 → down to (1,2)=8
    
- 8 <9 → down to (2,2)=9 → found
    

## 💻 Code (Java)

```java
// Staircase search: O(n + m)
public static boolean searchMatrixStaircase(int[][] mat, int target) {
    int n = mat.length;
    if (n == 0) return false;
    int m = mat[0].length;
    int i = 0, j = m - 1;
    while (i < n && j >= 0) {
        int val = mat[i][j];
        if (val == target) return true;
        else if (val > target) j--;
        else i++;
    }
    return false;
}
```

---

# ✅ Complexity Summary

- **Brute:** `O(n*m)` time
    
- **Better (per-row binary):** `O(n log m)` time
    
- **Optimal (staircase):** `O(n + m)` time, `O(1)` space — recommended
    

---

# ⚠ Edge Cases / Notes

- Matrix may be empty or have empty rows → handle `n==0` or `m==0`.
    
- Clarify matrix property in interview (row+col sorted vs flattened-sorted).
    
- If numbers large, comparisons remain fine; no overflow issues.
    

---

# Examples

- mat=[[1,4,7],[2,5,8],[3,6,9]], target=6 → true
    
- mat=[[1,2],[3,4]], target=5 → false
    

---

_File placement suggestion:_ `DSA/Matrix/03_Search in row and column sorted matrix.md`