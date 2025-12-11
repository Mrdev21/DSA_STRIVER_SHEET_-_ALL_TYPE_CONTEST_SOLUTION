# Rotate Matrix by 90° (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum ek **new matrix** banate hain.  
Rule:  
`new[j][n-1-i] = old[i][j]`  
Yani purane matrix ka element (i,j) naye matrix me rotated position me copy kar denge.  
Extra O(n²) space lagta hai.

---

## 📝 Dry Run
Original:
```
1 2 3
4 5 6
7 8 9
```

Mapping: new[j][2-i] = old[i][j]

Result:
```
7 4 1
8 5 2
9 6 3
```

---

## 💻 Code
```java
public static int[][] rotateBrute(int[][] mat) {
    int n = mat.length;
    int[][] res = new int[n][n];

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            res[j][n - 1 - i] = mat[i][j];
        }
    }

    return res;
}
```


# Rotate Matrix by 90° (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me bhi hum ek extra matrix banaate hain,  
bas mapping ko row-wise fill karte hain — same brute logic with structured iteration.  
Ye sirf brute ka clean version hai.  
Time: O(n²), space: O(n²).

---

## 📝 Dry Run
Matrix:
```
1 2
3 4
```

Rotation:
- old[0][0] = 1 → new[0][1]
- old[0][1] = 2 → new[1][1]
- old[1][0] = 3 → new[0][0]
- old[1][1] = 4 → new[1][0]

Result:
```
3 1
4 2
```

---

## 💻 Code
```java
public static int[][] rotateBetter(int[][] mat) {
    int n = mat.length;
    int[][] res = new int[n][n];
    int col = n - 1;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            res[j][col] = mat[i][j];
        }
        col--;
    }

    return res;
}
```






# Rotate Matrix by 90° (Optimal Approach — In-place)

## 🧠 Intuition (Hinglish)
Optimal trick:  
### Step 1 → **Transpose** matrix  
( rows ↔ columns swap )  

### Step 2 → **Reverse each row**  
Transpose ke baad har row ko reverse kar do → result is 90° clockwise rotation.  

Why this works?  
Transpose diagonally swap karta hai, aur row reversal se saare elements rotated position me shift ho jaate hain — bina extra space ke.

Time: O(n²)  
Space: O(1) (pure in-place)

---

## 📝 Dry Run

Original:
```
1 2 3
4 5 6
7 8 9
```

**Step 1: Transpose**
```
1 4 7
2 5 8
3 6 9
```

**Step 2: Reverse each row**
```
7 4 1
8 5 2
9 6 3
```

Final Answer:
```
7 4 1
8 5 2
9 6 3
```

---

## 💻 Code
```java
public static void rotateOptimal(int[][] mat) {
    int n = mat.length;

    // Step 1: Transpose
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            int temp = mat[i][j];
            mat[i][j] = mat[j][i];
            mat[j][i] = temp;
        }
    }

    // Step 2: Reverse each row
    for (int i = 0; i < n; i++) {
        int left = 0, right = n - 1;
        while (left < right) {
            int temp = mat[i][left];
            mat[i][left] = mat[i][right];
            mat[i][right] = temp;
            left++;
            right--;
        }
    }
}
```
