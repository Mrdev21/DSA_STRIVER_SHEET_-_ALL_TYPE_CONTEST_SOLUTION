
**Problem (short, Hinglish):** Tumhe ek binary matrix `mat[][]` diya gaya hai jisme har row **sorted hoti hai** (0s first then 1s). Tumhe woh row index return karna hai jisme **maximum number of 1s** ho. Agar multiple rows same count rakhti hain, return first such row.

---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute me hum har row ko pura scan karenge aur count karenge kitne 1s hain. Sabhi rows ka compare karke maximum 1s wali row return kar denge.

Simple hai lekin `O(n*m)` time lagta hai.

---

## 📝 Dry Run

mat = [  
[0,0,1,1],  
[0,1,1,1],  
[0,0,0,1]  
]

- row0: 2 ones
    
- row1: 3 ones → max so far
    
- row2: 1 one  
    Answer = row 1 (index 1)
    

---

## 💻 Code (Brute)

```java
public static int rowWithMax1sBrute(int[][] mat) {
    int n = mat.length, m = mat[0].length;
    int max1 = -1, ans = -1;

    for (int i = 0; i < n; i++) {
        int count = 0;
        for (int j = 0; j < m; j++) {
            if (mat[i][j] == 1) count++;
        }
        if (count > max1) {
            max1 = count;
            ans = i;
        }
    }
    return ans;
}
```

## Complexity

- Time: `O(n*m)`
    
- Space: `O(1)`
    

---

# Better Approach (Binary Search per row)

## 🧠 Intuition (Hinglish)

Row sorted hai, toh har row me (first_1_index) binary search se mil sakta hai. Phir number of 1s = `m - firstIndex`.

Total `O(n log m)`.

---

## 📝 Dry Run

Row = [0,0,1,1]

- Binary search → first 1 at index 2 → ones = 4-2 = 2  
    Row = [0,1,1,1]
    
- first 1 at index 1 → ones = 3
    

---

## 💻 Code (Better)

```java
public static int rowWithMax1sBetter(int[][] mat) {
    int n = mat.length, m = mat[0].length;
    int ans = -1, max1 = -1;

    for (int i = 0; i < n; i++) {
        int idx = firstOne(mat[i]);
        if (idx != -1) {
            int ones = m - idx;
            if (ones > max1) {
                max1 = ones;
                ans = i;
            }
        }
    }
    return ans;
}

private static int firstOne(int[] row) {
    int low = 0, high = row.length - 1;
    int pos = -1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (row[mid] == 1) {
            pos = mid;
            high = mid - 1;
        } else low = mid + 1;
    }
    return pos;
}
```

## Complexity

- Time: `O(n log m)`
    
- Space: `O(1)`
    

---

# Optimal Approach (Staircase Walk)

## 🧠 Intuition (Hinglish)

Since rows sorted hain, hum matrix ko **top-right corner** se traverse kar sakte hain:

- Agar cell = 1 → left move (1s zone explore)
    
- Agar cell = 0 → down move (next row try)
    

Yeh technique **one-pass** hai: worst-case `O(n+m)`.

Idea: jitni baar left jaoge, utne zyada 1s mil rahe, so record row.

---

## 📝 Dry Run

mat = [ [0,0,1,1], [0,1,1,1], [0,0,0,1] ]  
Start at top-right → (0,3)=1 → left → (0,2)=1 → left → (0,1)=0 → down → ...  
Row 1 gives max left moves = answer.

---

## 💻 Code (Optimal)

```java
public static int rowWithMax1sOptimal(int[][] mat) {
    int n = mat.length, m = mat[0].length;
    int i = 0, j = m - 1;
    int ans = -1;

    while (i < n && j >= 0) {
        if (mat[i][j] == 1) {
            ans = i;   // this row has more 1s than previous
            j--;       // move left
        } else {
            i++;       // move down
        }
    }
    return ans;
}
```

## Complexity

- Time: `O(n + m)`
    
- Space: `O(1)`
    

---

# ⚠ Edge Cases

- All rows have 0 → return -1
    
- Multiple rows same number of 1s → return first
    
- Matrix always sorted row-wise
    

---

# Examples

- mat=[[0,0,1],[0,1,1],[0,0,0]] → row 1
    
- mat=[[0,0],[0,0]] → -1
    

---

_File placement suggestion:_ `DSA/Matrix/01_Find the row with maximum number of 1s.md`