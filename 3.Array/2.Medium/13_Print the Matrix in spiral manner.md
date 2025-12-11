

# Print the Matrix in Spiral Manner (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute approach me hum matrix ko spiral order me print karne ke liye ek **visited** boolean matrix use karte hain.
Har step pe current direction (right, down, left, up) follow karte hue aage badho.
Agar next cell out-of-bounds ya already visited ho jaye to direction change karlo.
Ye simple simulation hai — extra O(m*n) space for `visited`, time O(m*n).

---

## 📝 Dry Run
Matrix:
```
1  2  3
4  5  6
7  8  9
```

Start at (0,0), dir = right:
- (0,0) → 1
- (0,1) → 2
- (0,2) → 3
next right is out → change dir = down
- (1,2) → 6
- (2,2) → 9
next down out → change dir = left
- (2,1) → 8
- (2,0) → 7
next left out/visited → change dir = up
- (1,0) → 4
next up visited → change dir = right
- (1,1) → 5
Done → spiral = [1,2,3,6,9,8,7,4,5]

---

## 💻 Code
```java
// Brute: simulate with visited matrix and direction changes
public static List<Integer> spiralBrute(int[][] mat) {
    List<Integer> res = new ArrayList<>();
    if (mat == null || mat.length == 0) return res;

    int m = mat.length;
    int n = mat[0].length;
    boolean[][] visited = new boolean[m][n];

    // directions: right, down, left, up
    int[] dr = {0, 1, 0, -1};
    int[] dc = {1, 0, -1, 0};
    int dir = 0; // start moving right

    int r = 0, c = 0;
    for (int k = 0; k < m * n; k++) {
        res.add(mat[r][c]);
        visited[r][c] = true;

        int nr = r + dr[dir];
        int nc = c + dc[dir];

        if (nr < 0 || nr >= m || nc < 0 || nc >= n || visited[nr][nc]) {
            // change direction
            dir = (dir + 1) % 4;
            nr = r + dr[dir];
            nc = c + dc[dir];
        }

        r = nr;
        c = nc;
    }

    return res;
}
```



# Print the Matrix in Spiral Manner (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum `top, bottom, left, right` boundaries maintain karte hain.
Har boundary ko traverse karo:
- left→right on top row, then top++
- top→bottom on right col, then right--
- right→left on bottom row, then bottom--
- bottom→top on left col, then left++
Is tarah boundaries shrink karte rahenge jab tak sab elements print na ho jaye.
Space O(1) extra (output list excluded), time O(m*n). Simple aur common solution.

---

## 📝 Dry Run
Matrix:
```
1  2  3  4
5  6  7  8
9 10 11 12
```

Initial: top=0, bottom=2, left=0, right=3

- top row (left→right): 1,2,3,4  → top=1
- right col (top→bottom): 8,12  → right=2
- bottom row (right→left): 11,10,9 → bottom=1
- left col (bottom→top): 5 → left=1

Now top=1,bottom=1,left=1,right=2
- top row: 6,7 → top=2 (loop ends)

Spiral → [1,2,3,4,8,12,11,10,9,5,6,7]

---

## 💻 Code
```java
// Better: boundary approach using top/bottom/left/right
public static List<Integer> spiralBetter(int[][] mat) {
    List<Integer> res = new ArrayList<>();
    if (mat == null || mat.length == 0) return res;

    int top = 0;
    int bottom = mat.length - 1;
    int left = 0;
    int right = mat[0].length - 1;

    while (top <= bottom && left <= right) {
        // traverse top row
        for (int j = left; j <= right; j++) res.add(mat[top][j]);
        top++;

        // traverse right column
        for (int i = top; i <= bottom; i++) res.add(mat[i][right]);
        right--;

        if (top <= bottom) {
            // traverse bottom row
            for (int j = right; j >= left; j--) res.add(mat[bottom][j]);
            bottom--;
        }

        if (left <= right) {
            // traverse left column
            for (int i = bottom; i >= top; i--) res.add(mat[i][left]);
            left++;
        }
    }

    return res;
}
```


# Print the Matrix in Spiral Manner (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum `top, bottom, left, right` boundaries maintain karte hain.
Har boundary ko traverse karo:
- left→right on top row, then top++
- top→bottom on right col, then right--
- right→left on bottom row, then bottom--
- bottom→top on left col, then left++
Is tarah boundaries shrink karte rahenge jab tak sab elements print na ho jaye.
Space O(1) extra (output list excluded), time O(m*n). Simple aur common solution.

---

## 📝 Dry Run
Matrix:
```
1  2  3  4
5  6  7  8
9 10 11 12
```

Initial: top=0, bottom=2, left=0, right=3

- top row (left→right): 1,2,3,4  → top=1
- right col (top→bottom): 8,12  → right=2
- bottom row (right→left): 11,10,9 → bottom=1
- left col (bottom→top): 5 → left=1

Now top=1,bottom=1,left=1,right=2
- top row: 6,7 → top=2 (loop ends)

Spiral → [1,2,3,4,8,12,11,10,9,5,6,7]

---

## 💻 Code
```java
// Better: boundary approach using top/bottom/left/right
public static List<Integer> spiralBetter(int[][] mat) {
    List<Integer> res = new ArrayList<>();
    if (mat == null || mat.length == 0) return res;

    int top = 0;
    int bottom = mat.length - 1;
    int left = 0;
    int right = mat[0].length - 1;

    while (top <= bottom && left <= right) {
        // traverse top row
        for (int j = left; j <= right; j++) res.add(mat[top][j]);
        top++;

        // traverse right column
        for (int i = top; i <= bottom; i++) res.add(mat[i][right]);
        right--;

        if (top <= bottom) {
            // traverse bottom row
            for (int j = right; j >= left; j--) res.add(mat[bottom][j]);
            bottom--;
        }

        if (left <= right) {
            // traverse left column
            for (int i = bottom; i >= top; i--) res.add(mat[i][left]);
            left++;
        }
    }

    return res;
}
```


