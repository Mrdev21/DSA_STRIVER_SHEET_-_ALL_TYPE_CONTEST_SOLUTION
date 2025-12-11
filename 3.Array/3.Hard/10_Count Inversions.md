
# Count Inversions — Brute Approach (Approach: Pairwise check)

## 🧠 Intuition (Hinglish)
Brute force simple hai: har pair (i < j) check karo — agar `arr[i] > arr[j]` toh ek inversion.  
Direct aur easy samajh aata hai, par time O(n²) hai — small inputs ke liye theek.

---

## 📝 Dry Run
Array = [2, 4, 1, 3, 5]

Pairs:
(2,4) no, (2,1) yes → 1  
(2,3) no, (2,5) no  
(4,1) yes → 2  
(4,3) yes → 3  
(4,5) no  
(1,3) no, (1,5) no  
(3,5) no

Total inversions = **3**

---

## 💻 Code
```java
// Brute: O(n^2)
public static long countInversionsBrute(int[] arr) {
    int n = arr.length;
    long count = 0;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (arr[i] > arr[j]) count++;
        }
    }
    return count;
}
```


# Count Inversions — Better Approach (Approach: Binary Indexed Tree / Fenwick Tree)

## 🧠 Intuition (Hinglish)
Better approach me BIT (Fenwick) use karenge with **coordinate compression**:
- Idea: iterate array from right → for current `x` count how many elements already seen that are < x (or <= x-1 depending).  
- Using BIT we can update counts and query prefix sums in O(log N).  
- Steps:
  1. Coordinate-compress values to 1..M (M ≤ n).
  2. Iterate i from n-1 down to 0:
     - inversions += query(index_of(arr[i]) - 1)  // count elements smaller to right
     - update(index_of(arr[i]), +1)
- Time O(n log n), Space O(n).

Note: depending on definition you may query (val-1) for strictly smaller; for counting arr[i] > arr[j] we count numbers < arr[i] to right.

---

## 📝 Dry Run
Array = [2, 4, 1, 3, 5]
Compression mapping (sorted uniq): [1,2,3,4,5] → indices same.

Iterate from right:
- i=4 val=5 → query(4)=0 → inv+=0 ; update(5)
- i=3 val=3 → query(2)=0 → inv+=0 ; update(3)
- i=2 val=1 → query(0)=0 → inv+=0 ; update(1)
- i=1 val=4 → query(3)= (counts of <=3) = counts at indices 1 & 3 → there are 2 (1 and 3) → inversions += 2
- i=0 val=2 → query(1)= (counts <=1) = 1 (the 1) → inversions +=1
Total = 0+0+0+2+1 = **3**

---

## 💻 Code
```java
// Better: BIT with coordinate compression. O(n log n)
public static class Fenwick {
    int n;
    int[] bit;
    public Fenwick(int n) {
        this.n = n;
        bit = new int[n + 1];
    }
    public void add(int idx, int delta) {
        for (; idx <= n; idx += idx & -idx) bit[idx] += delta;
    }
    public int sum(int idx) {
        int s = 0;
        for (; idx > 0; idx -= idx & -idx) s += bit[idx];
        return s;
    }
}

public static long countInversionsBIT(int[] arr) {
    int n = arr.length;
    // coordinate compression
    int[] vals = Arrays.copyOf(arr, n);
    Arrays.sort(vals);
    Map<Integer, Integer> idx = new HashMap<>();
    int rank = 0;
    for (int v : vals) {
        if (!idx.containsKey(v)) idx.put(v, ++rank);
    }

    Fenwick fw = new Fenwick(rank);
    long inv = 0;
    // iterate from right, count numbers < arr[i] already seen
    for (int i = n - 1; i >= 0; i--) {
        int r = idx.get(arr[i]);
        // count of elements strictly less than current = sum(r-1)
        inv += fw.sum(r - 1);
        fw.add(r, 1);
    }
    return inv;
}
```

# Count Inversions — Optimal Approach (Approach: Modified Merge Sort)

## 🧠 Intuition (Hinglish)
Classic optimal method: merge-sort based counting.
- During merge of two sorted halves, whenever left[i] > right[j], then **all remaining elements** in left from i..end form inversions with right[j] (because left subarray is sorted).
- Count those `(mid - i + 1)` and continue merge.
- Time O(n log n), Space O(n) for temp array. Simple, fast, and standard.

---

## 📝 Dry Run
Array = [2, 4, 1, 3, 5]

Divide:
- left [2,4], right [1,3,5]
Merge steps count:
- merge left [2,4] → no inversions inside (0)
- merge right [1,3,5] → none (0)
- merge [2,4] and [1,3,5]:
  - compare 2 and 1 → 2>1 → inversions += (2 elements left: 2 and 4) ? actually left has 2 elements starting at i -> inversions += 2? careful: at moment left = [2,4], right pointer at 1 -> inversions += size(left)-i = 2 → inv=2
  - next compare 2 and 3 → 2<=3 -> place 2
  - compare 4 and 3 -> 4>3 -> inversions += remaining in left (now 1 element: 4) -> inv +=1 => total 3
Result total inversions = **3**

---

## 💻 Code
```java
// Optimal: Merge sort counting. O(n log n), uses O(n) temp array.
public static long countInversionsMergeSort(int[] arr) {
    if (arr == null || arr.length <= 1) return 0L;
    int[] tmp = new int[arr.length];
    return mergeCount(arr, tmp, 0, arr.length - 1);
}

private static long mergeCount(int[] a, int[] tmp, int l, int r) {
    if (l >= r) return 0L;
    int mid = l + (r - l) / 2;
    long cnt = 0;
    cnt += mergeCount(a, tmp, l, mid);
    cnt += mergeCount(a, tmp, mid + 1, r);

    int i = l, j = mid + 1, k = l;
    while (i <= mid && j <= r) {
        if (a[i] <= a[j]) {
            tmp[k++] = a[i++];
        } else {
            // a[i] > a[j] -> inversions for all remaining in left
            tmp[k++] = a[j++];
            cnt += (mid - i + 1);
        }
    }
    while (i <= mid) tmp[k++] = a[i++];
    while (j <= r) tmp[k++] = a[j++];
    for (int p = l; p <= r; p++) a[p] = tmp[p];
    return cnt;
}
```






