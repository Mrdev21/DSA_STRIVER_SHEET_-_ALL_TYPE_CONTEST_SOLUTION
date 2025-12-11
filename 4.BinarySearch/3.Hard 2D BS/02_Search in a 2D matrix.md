
**Problem (short, Hinglish):** Tumhe ek 2D matrix di hui hai aur ek target value. Do popular variants hoti hain:

- **Variant A (Flattened-sorted):** Har row sorted hai and `first element of row i > last element of row i-1` (purely increasing when flattened). Check karo agar target matrix me present hai.
    
- **Variant B (Row & Column sorted):** Har row sorted hai left→right aur har column bhi sorted hai top→down (but no relation between end of a row & start of next). Check karo agar target present hai.
    

Main dono variants ke solutions dunga (brute → better → optimal), kyunki interview me dono questions commonly aate hain.

---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute me bas pure matrix ko scan karo row by row and compare each cell with target. Simple and foolproof.

## 📝 Dry Run

matrix = [[1,3,5],[7,9,11],[13,15,17]], target=9

- scan -> found at (1,1)
    

## 💻 Code (Java)

```java
// Brute: O(n*m) scan
public static boolean searchMatrixBrute(int[][] mat, int target) {
    int n = mat.length, m = mat[0].length;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (mat[i][j] == target) return true;
        }
    }
    return false;
}
```

## Complexity

- Time: `O(n*m)`
    
- Space: `O(1)`
    

---

# Better Approach (Variant A: Binary Search on Flattened Array)

## 🧠 Intuition (Hinglish)

Agar matrix is Flattened-sorted (Variant A), to tum matrix ko virtually ek sorted 1D array samajh sakte ho. Index `k` maps to `(k / m, k % m)`. Binary search kar do over `0..n*m-1`.

## 📝 Dry Run

mat = [[1,3,5],[7,9,11],[13,15,17]], target=9

- n=3,m=3 -> search over 0..8
    
- mid=4 -> index (1,1)=9 -> found
    

## 💻 Code (Java)

```java
// Variant A: binary search on 1D index -> O(log(n*m))
public static boolean searchMatrixBinary(int[][] mat, int target) {
    int n = mat.length, m = mat[0].length;
    int low = 0, high = n * m - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        int val = mat[mid / m][mid % m];
        if (val == target) return true;
        else if (val < target) low = mid + 1;
        else high = mid - 1;
    }
    return false;
}
```

## Complexity

- Time: `O(log(n*m))` = `O(log n + log m)`
    
- Space: `O(1)`
    

---

# Optimal Approach (Variant B: Staircase Search for Row & Column Sorted)

## 🧠 Intuition (Hinglish)

Agar matrix rows aur columns dono sorted hain (Variant B), best trick hai **start at top-right** (or bottom-left).

- If current == target → found
    
- If current > target → move left (reduce value)
    
- If current < target → move down (increase value)
    

Ye monotonic 2D walk `O(n + m)` time me kam karta hai and space `O(1)`.

## 📝 Dry Run

mat = [[1,4,7,11],[2,5,8,12],[3,6,9,16]], target=9

- start (0,3)=11 >9 -> left
    
- (0,2)=7 <9 -> down
    
- (1,2)=8 <9 -> down
    
- (2,2)=9 -> found
    

## 💻 Code (Java)

```java
// Variant B: staircase search O(n + m)
public static boolean searchMatrixStaircase(int[][] mat, int target) {
    int n = mat.length, m = mat[0].length;
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

## Complexity

- Time: `O(n + m)`
    
- Space: `O(1)`
    

---

# ⚠ Edge Cases / Notes

- Always clarify the matrix property in interview: Variant A (flattened sorted) vs Variant B (row & column sorted) — solutions differ.
    
- Empty matrix or empty rows → return false.
    
- For Variant A ensure no integer overflow when computing `n*m` for extremely large matrices (use long if needed).
    

---

# Examples

- Variant A: mat=[[1,3,5],[7,9,11]], target=5 → true (binary search)
    
- Variant B: mat=[[1,4,7],[2,5,8],[3,6,9]], target=6 → true (staircase)
    

---

_File placement suggestion:_ `DSA/Matrix/02_Search in a 2D matrix.md`