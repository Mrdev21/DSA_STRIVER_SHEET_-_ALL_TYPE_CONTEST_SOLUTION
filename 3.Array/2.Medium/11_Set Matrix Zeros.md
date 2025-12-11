
# Set Matrix Zeros (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute approach me hum simple idea use karte hain:  
Jab kisi cell me 0 mile, uski row aur column ko zero karna hai — lekin agar same matrix me turant overwrite karenge to naye zeros doosre cells ka effect badha denge.  
Isliye brute me hum pehle ek **deep copy** bana lenge (original ko copy kar ke), aur copy me jo zeros hain unke corresponding row & column original matrix me set kar denge.

---

## 📝 Dry Run
Matrix:
```
[1, 1, 1]
[1, 0, 1]
[1, 1, 1]
```

Copy me zero at (1,1) milega → original me row 1 and col 1 ko zero karo:

Result:
```
[1, 0, 1]
[0, 0, 0]
[1, 0, 1]
```

---

## 💻 Code
```java
public static void setZeroesBrute(int[][] mat) {
    if (mat == null || mat.length == 0) return;
    int m = mat.length;
    int n = mat[0].length;

    // deep copy
    int[][] copy = new int[m][n];
    for (int i = 0; i < m; i++) {
        System.arraycopy(mat[i], 0, copy[i], 0, n);
    }

    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (copy[i][j] == 0) {
                // set entire row i to 0 in original
                for (int c = 0; c < n; c++) mat[i][c] = 0;
                // set entire column j to 0 in original
                for (int r = 0; r < m; r++) mat[r][j] = 0;
            }
        }
    }
}
```




# Set Matrix Zeros (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum extra space use kar ke rows aur columns ko mark karenge:  
Do boolean arrays bana lo — `rows[m]` aur `cols[n]`.  
Pehle pass me jaha zero mile, `rows[i]=true`, `cols[j]=true` kar do.  
Doosre pass me jo cells un marked rows/cols me aate hain unko zero kar do.  
Time O(m*n), space O(m + n) — simple aur safe.

---

## 📝 Dry Run
Matrix:
```
[0, 1, 2, 0]
[3, 4, 5, 2]
[1, 3, 1, 5]
```

Pass1: zeros at (0,0) and (0,3) → rows[0]=true, cols[0]=true, cols[3]=true

Pass2: set any cell with rows[i] or cols[j] true → result:

```
[0, 0, 0, 0]
[0, 4, 5, 0]
[0, 3, 1, 0]
```

---

## 💻 Code
```java
public static void setZeroesBetter(int[][] mat) {
    if (mat == null || mat.length == 0) return;
    int m = mat.length;
    int n = mat[0].length;

    boolean[] rows = new boolean[m];
    boolean[] cols = new boolean[n];

    // mark rows and cols
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (mat[i][j] == 0) {
                rows[i] = true;
                cols[j] = true;
            }
        }
    }

    // set zeros using marks
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (rows[i] || cols[j]) mat[i][j] = 0;
        }
    }
}
```


# Set Matrix Zeros (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal in-place approach first-row/first-column marker trick use karta hai (O(1) extra space):
- Use first row and first column as markers to indicate whether that row/col should be zeroed.
- Pehle determine karo ki **first row** me koi zero tha (flag `firstRowZero`) aur **first col** me koi zero tha (`firstColZero`).
- Fir matrix ke rest elements (1..m-1,1..n-1) me agar mat[i][j]==0 ho → set mat[i][0]=0 aur mat[0][j]=0 (markers).
- Phir markers ke basis par rows/cols ko zero karo (skip first row/col initially).
- Finally `firstRowZero` ya `firstColZero` ke hisaab se first row/col ko zero karo agar required ho.
Isse extra O(1) space aur time O(m*n).

---

## 📝 Dry Run
Matrix:
```
[1, 1, 1]
[1, 0, 1]
[1, 1, 1]
```

Step:
- firstRowZero=false, firstColZero=false
- Use inner cells: (1,1)==0 → mark mat[1][0]=0, mat[0][1]=0 => matrix becomes:
  ```
  [1, 0, 1]
  [0, 0, 1]
  [1, 1, 1]
  ```
- Now zero rows (based on mat[i][0]): row1 -> zero
- Zero cols (based on mat[0][j]): col1 -> zero
- firstRow/firstCol not zero → no change
Final:
```
[1, 0, 1]
[0, 0, 0]
[1, 0, 1]
```

(Example with first row/col having zeros will trigger final zeroing of those.)

---

## 💻 Code
```java
public static void setZeroesOptimal(int[][] mat) {
    if (mat == null || mat.length == 0) return;
    int m = mat.length;
    int n = mat[0].length;

    boolean firstRowZero = false;
    boolean firstColZero = false;

    // check first row
    for (int j = 0; j < n; j++) {
        if (mat[0][j] == 0) {
            firstRowZero = true;
            break;
        }
    }

    // check first column
    for (int i = 0; i < m; i++) {
        if (mat[i][0] == 0) {
            firstColZero = true;
            break;
        }
    }

    // use first row and column as markers
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            if (mat[i][j] == 0) {
                mat[i][0] = 0;
                mat[0][j] = 0;
            }
        }
    }

    // zero rows based on markers
    for (int i = 1; i < m; i++) {
        if (mat[i][0] == 0) {
            for (int j = 1; j < n; j++) mat[i][j] = 0;
        }
    }

    // zero columns based on markers
    for (int j = 1; j < n; j++) {
        if (mat[0][j] == 0) {
            for (int i = 1; i < m; i++) mat[i][j] = 0;
        }
    }

    // finally zero first row/col if needed
    if (firstRowZero) {
        for (int j = 0; j < n; j++) mat[0][j] = 0;
    }
    if (firstColZero) {
        for (int i = 0; i < m; i++) mat[i][0] = 0;
    }
}
```


