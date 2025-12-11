

**Problem (short, Hinglish):** Tumhe ek 2D matrix `mat[][]` di hui hai. Ek element ko **peak** bolte hain agar woh apne 4-directional neighbors (up, down, left, right) se **strictly greater** ho. Edge cells ke missing neighbors ko ignore karo. Tumhe matrix me se **koi bhi ek peak element ka index** return karna hai. (Goal: Kisi bhi peak ko efficiently dhoondo — guaranteed nahi har matrix me multiple peaks ho sakte hain, par at least one peak hamesha exist karta hai under this definition.)

---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute me har cell ko check karo aur dekh lo kya woh up, down, left, right sab se bada hai. Agar haan → return index. Simple scan.

## 📝 Dry Run

mat = [  
[10, 8, 10],  
[14,13,12],  
[15, 9,11]  
]  
Scan:

- (0,0)=10 → right 8 smaller, down 14 larger → not peak
    
- (0,1)=8 -> neighbors have larger → not
    
- (0,2)=10 -> down 12 larger → not
    
- (1,0)=14 -> up 10 smaller, left none, right 13 smaller, down 15 larger → not
    
- (1,1)=13 -> up 8 smaller, left14 larger -> not
    
- (1,2)=12 -> up10 smaller, down11 smaller, left13 larger -> not
    
- (2,0)=15 -> up14 smaller, right9 smaller -> **peak found at (2,0)**
    

## 💻 Code (Java)

```java
// Brute: O(n*m) scan, check 4-neighbors
public static int[] findPeak2DBrute(int[][] mat) {
    int n = mat.length;
    if (n == 0) return new int[] {-1, -1};
    int m = mat[0].length;

    int[] dirs = {-1,0,1,0,-1}; // to iterate neighbors
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            boolean isPeak = true;
            int val = mat[i][j];
            for (int d = 0; d < 4; d++) {
                int ni = i + dirs[d];
                int nj = j + dirs[d+1];
                if (ni >= 0 && ni < n && nj >= 0 && nj < m) {
                    if (mat[ni][nj] >= val) { isPeak = false; break; }
                }
            }
            if (isPeak) return new int[] {i, j};
        }
    }
    return new int[] {-1, -1};
}
```

## Complexity

- Time: `O(n * m)`
    
- Space: `O(1)`
    

---

# Better Approach (Greedy Ascent from Middle Column)

## 🧠 Intuition (Hinglish)

Ek efficient trick: pick a column (usually middle), find the global maximum in that column (row index r). Check its left and right neighbors:

- Agar column-max element >= both left and right neighbors (or neighbors absent) → woh 2D-peak hoga (kyunki it's column max and also larger than horizontal neighbors).
    
- Agar left neighbor larger → move left (search left half)
    
- Agar right neighbor larger → move right (search right half)
    

Isme hum gradually columns ko half karte hue peak ki taraf badhte hain — that's binary-search-like.

## 📝 Dry Run

mat same as previous. n=3,m=3

- pick midCol=1 → column values [8,13,9], max at r=1 (13)
    
- left of (1,1) is 14 which is larger → move to left half (col range 0..0)
    
- midCol=0 → column [10,14,15], max at r=2 (15)
    
- check neighbors: right of (2,0) is 9 smaller, up is 14 smaller → peak at (2,0)
    

## 💻 Code (Java)

```java
// Better: column-wise binary-style search O(n log m)
public static int[] findPeak2DBetter(int[][] mat) {
    int n = mat.length;
    if (n == 0) return new int[] {-1,-1};
    int m = mat[0].length;

    int left = 0, right = m - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        int maxRow = maxRowInCol(mat, mid);
        int val = mat[maxRow][mid];
        int leftVal = (mid - 1 >= 0) ? mat[maxRow][mid - 1] : Integer.MIN_VALUE;
        int rightVal = (mid + 1 < m) ? mat[maxRow][mid + 1] : Integer.MIN_VALUE;

        if (val >= leftVal && val >= rightVal) {
            return new int[] {maxRow, mid};
        } else if (leftVal > val) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    return new int[] {-1,-1};
}

private static int maxRowInCol(int[][] mat, int col) {
    int n = mat.length, maxR = 0;
    for (int i = 1; i < n; i++) {
        if (mat[i][col] > mat[maxR][col]) maxR = i;
    }
    return maxR;
}
```

## Complexity

- Time: `O(n * log m)` (find max in column O(n) repeated for O(log m) column probes)
    
- Space: `O(1)`
    

---

# Optimal Approach (Divide & Conquer on Smaller Dimension)

## 🧠 Intuition (Hinglish)

Better approach can be tuned: always binary-search on the **smaller** dimension (columns or rows) to make it `O(min(n,m) * log(max(n,m)))`. Practically, for tall matrices search on columns; for wide matrices search on rows by symmetric logic.

Implementation: if `m <= n` do column-binary as above, else transpose approach and run row-binary.

## 📝 Dry Run

For a wide matrix prefer searching rows instead — same idea mirrored.

## 💻 Code (Java)

```java
// Optimal: search on smaller dimension to minimize work
public static int[] findPeak2DOptimal(int[][] mat) {
    int n = mat.length; if (n == 0) return new int[] {-1,-1};
    int m = mat[0].length;
    if (m <= n) return findPeak2DBetter(mat); // search cols

    // else search on rows (mirror logic)
    int top = 0, bottom = n - 1;
    while (top <= bottom) {
        int mid = top + (bottom - top) / 2;
        int maxCol = maxColInRow(mat, mid);
        int val = mat[mid][maxCol];
        int upVal = (mid - 1 >= 0) ? mat[mid - 1][maxCol] : Integer.MIN_VALUE;
        int downVal = (mid + 1 < n) ? mat[mid + 1][maxCol] : Integer.MIN_VALUE;

        if (val >= upVal && val >= downVal) return new int[] {mid, maxCol};
        else if (upVal > val) bottom = mid - 1;
        else top = mid + 1;
    }
    return new int[] {-1,-1};
}

private static int maxColInRow(int[][] mat, int row) {
    int m = mat[0].length, maxC = 0;
    for (int j = 1; j < m; j++) if (mat[row][j] > mat[row][maxC]) maxC = j;
    return maxC;
}
```

## Complexity

- Time: `O(min(n,m) * log(max(n,m)))`
    
- Space: `O(1)`
    

---

# ✅ Notes / Edge Cases

- Peak definition is strict (> neighbors). If non-strict (>=) changes, handle equal neighbors carefully.
    
- Matrix with all equal values → no strict peak; return any index per problem constraints or handle as -1.
    
- Use sentinels `Integer.MIN_VALUE` for missing neighbors.
    

---

# Examples

- mat = [[10,8,10],[14,13,12],[15,9,11]] → one possible peak at (2,0) with value 15
    
- mat = [[1]] → (0,0)
    

---

_File placement suggestion:_ `DSA/Matrix/04_Find Peak Element (2D).md`