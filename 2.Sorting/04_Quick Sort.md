### 🧠 Intuition (Hinglish)
Choose pivot, partition array so that left ≤ pivot ≤ right, phir recursively sort halves. Quick average fast, but worst-case O(n²).

### ⚙️ Approach (Lomuto)
- Choose pivot (e.g., arr[high]).
- i = low - 1;
- for j = low..high-1: if arr[j] <= pivot → i++; swap arr[i], arr[j]
- swap arr[i+1], arr[high]; pivot at i+1

### 📝 Dry Run
arr=[4,2,3,1], pivot=1
partition -> pivot becomes first element after swaps -> then recursive sorts.

### 💻 Code
```java
public static void quickSortLomuto(int[] arr) {
    quickRec(arr, 0, arr.length - 1);
}
private static void quickRec(int[] a, int l, int r) {
    if (l >= r) return;
    int p = partitionLomuto(a, l, r);
    quickRec(a, l, p - 1);
    quickRec(a, p + 1, r);
}
private static int partitionLomuto(int[] a, int l, int r) {
    int pivot = a[r];
    int i = l - 1;
    for (int j = l; j < r; j++) {
        if (a[j] <= pivot) {
            i++;
            int t = a[i]; a[i] = a[j]; a[j] = t;
        }
    }
    int t = a[i + 1]; a[i + 1] = a[r]; a[r] = t;
    return i + 1;
}
```

### ⏱ Complexity
- Average: O(n log n), Worst: O(n²) (if bad pivot)  
- Space: O(log n) recursion average  
- In-place: Yes (O(1) aux)  
- Not stable (by default)  
- Use: practical with randomized pivot or median-of-three to avoid worst-case.

---

## 7) Quick Sort (Hoare partition)

### 🧠 Intuition
Hoare partition generally does fewer swaps and can be faster in practice. Picks pivot (often middle), moves i/j pointers inward swapping elements that are on wrong side.

### ⚙️ Approach
- pivot = a[(l+r)/2]; i = l-1; j = r+1;
- while true: do i++ while a[i] < pivot; do j-- while a[j] > pivot; if i>=j return j; swap a[i], a[j].
- Recurse on [l..p] and [p+1..r] where p is returned index.

### 💻 Code
```java
public static void quickSortHoare(int[] arr) {
    quickHoare(arr, 0, arr.length - 1);
}
private static void quickHoare(int[] a, int l, int r) {
    if (l >= r) return;
    int p = partitionHoare(a, l, r);
    quickHoare(a, l, p);
    quickHoare(a, p + 1, r);
}
private static int partitionHoare(int[] a, int l, int r) {
    int pivot = a[l + (r - l) / 2];
    int i = l - 1;
    int j = r + 1;
    while (true) {
        do { i++; } while (a[i] < pivot);
        do { j--; } while (a[j] > pivot);
        if (i >= j) return j;
        int t = a[i]; a[i] = a[j]; a[j] = t;
    }
}
```

### ⏱ Complexity
- Average O(n log n), worst O(n²)  
- Often faster than Lomuto in practice  
- Not stable.

---
