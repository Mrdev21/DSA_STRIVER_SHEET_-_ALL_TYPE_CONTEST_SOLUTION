
**Problem (short, Hinglish):** Tumhe ek row-wise sorted matrix `mat` di hui hai (har row sorted ascending). Tumhe matrix ka **median** nikalna hai — i.e., woh value `x` jiske liye number of elements `<= x` at least `(R*C + 1)/2`. Return the median value (assume integer matrix).

---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute me saare elements ko ek list me nikaal lo, sort kar do, aur middle element return kar lo. Simple and correct.

## 📝 Dry Run

mat = [[1,3,5],[2,6,9],[3,6,9]] (R=3,C=3) → flattened = [1,2,3,3,5,6,6,9,9]  
median index = (9+1)/2 = 5th smallest (1-based) → value = 5

## 💻 Code (Java)

```java
// Brute: flatten + sort
public static int matrixMedianBrute(int[][] mat) {
    int n = mat.length;
    int m = mat[0].length;
    int[] all = new int[n * m];
    int idx = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) all[idx++] = mat[i][j];
    }
    Arrays.sort(all);
    int mid = (n * m + 1) / 2; // 1-based
    return all[mid - 1];
}
```

## Complexity

- Time: `O((R*C) log(R*C))` due to sorting
    
- Space: `O(R*C)` for flattened array
    

---

# Better Approach (Min-Heap / K-way merge)

## 🧠 Intuition (Hinglish)

Matrix rows sorted hai → hum `k`-way merge use kar sakte hain using a min-heap (priority queue). Push first element of each row (value, row, col) into PQ and pop smallest repeatedly until we reach the median position.

Yeh space `O(R)` and time about `O(medianIndex * log R)` — better than full sort when rows are many but total elements large.

## 📝 Dry Run

mat = [[1,3,5],[2,6,9],[3,6,9]]  
Push (1,row0),(2,row1),(3,row2)  
Pop three times etc until 5th popped -> 5

## 💻 Code (Java)

```java
// Better: min-heap k-way merge to get median
static class Node {
    int val, r, c;
    Node(int v, int r, int c) { val = v; this.r = r; this.c = c; }
}

public static int matrixMedianHeap(int[][] mat) {
    int R = mat.length, C = mat[0].length;
    PriorityQueue<Node> pq = new PriorityQueue<>((a,b) -> Integer.compare(a.val, b.val));
    for (int i = 0; i < R; i++) pq.add(new Node(mat[i][0], i, 0));

    int target = (R * C + 1) / 2;
    int count = 0, ans = -1;
    while (!pq.isEmpty()) {
        Node cur = pq.poll();
        count++;
        if (count == target) { ans = cur.val; break; }
        if (cur.c + 1 < C) pq.add(new Node(mat[cur.r][cur.c + 1], cur.r, cur.c + 1));
    }
    return ans;
}
```

## Complexity

- Time: `O(target * log R)` where `target = (R*C+1)/2`
    
- Space: `O(R)` for PQ
    

---

# Optimal Approach (Binary Search on Value)

## 🧠 Intuition (Hinglish)

Since rows are sorted, we can binary search on the **value range** (not indices). Let `low = min element` and `high = max element` (i.e., across matrix). For mid = (low+high)/2, count how many elements `<= mid` by doing `upper_bound` (binary search) on each row. If `count < desired` → raise `low`; else reduce `high`.

Stop when low crosses high; `low` (or `high`) will be median.

## 📝 Dry Run

mat = [[1,3,5],[2,6,9],[3,6,9]]  
low=1, high=9  
mid=5 -> count of <=5 across rows = row0:3, row1:1, row2:1 => total=5 target=5 -> high=5  
mid=3 -> counts => row0:2,row1:1,row2:2 => total=5 -> high=3 -> eventually low settles at 5 (final answer)

## 💻 Code (Java)

```java
// Optimal: binary search on value range
public static int matrixMedianOptimal(int[][] mat) {
    int R = mat.length, C = mat[0].length;
    int low = Integer.MAX_VALUE, high = Integer.MIN_VALUE;
    for (int i = 0; i < R; i++) {
        low = Math.min(low, mat[i][0]);
        high = Math.max(high, mat[i][C - 1]);
    }

    int desired = (R * C + 1) / 2;
    while (low < high) {
        int mid = low + (high - low) / 2;
        int cnt = 0;
        for (int i = 0; i < R; i++) {
            cnt += countLessEqual(mat[i], mid);
        }
        if (cnt < desired) low = mid + 1;
        else high = mid;
    }
    return low;
}

private static int countLessEqual(int[] row, int x) {
    int l = 0, r = row.length; // upper_bound style
    while (l < r) {
        int m = l + (r - l) / 2;
        if (row[m] <= x) l = m + 1;
        else r = m;
    }
    return l;
}
```

## Complexity

- Time: `O( log(range) * R * log C )` where `range = maxVal - minVal` (log(range) iterations, each row binary search)
    
- Space: `O(1)`
    

---

# ✅ Notes / Edge Cases

- Works for odd total elements; for even total elements this definition returns the lower-median (common in matrix-median problems). Clarify in interview if they want average of two middle values.
    
- Rows must be sorted. If not sorted, use brute flatten+sort.
    
- Use `int` carefulness if values near limits; for `low/high` you may want `long` if necessary.
    

---

# Examples

- mat=[[1,3,5],[2,6,9],[3,6,9]] → median = 5
    
- mat=[[1,2,3],[4,5,6]] → median = 3 (total 6 -> (6+1)/2 = 3rd smallest)
    

---

_File placement suggestion:_ `DSA/Matrix/05_Matrix Median.md`