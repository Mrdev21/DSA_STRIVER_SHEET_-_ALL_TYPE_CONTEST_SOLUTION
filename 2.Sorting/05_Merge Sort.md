## 5) Merge Sort

### 🧠 Intuition (Hinglish)
Divide & conquer: array ko half-half divide karo, sort kar ke merge kar do. Merge step sorted halves ko O(n) me combine karta hai.

### ⚙️ Approach
- Recursively split until size 1.
- Merge two sorted halves using temp array.

### 📝 Dry Run (high-level)
arr=[4,2,3,1]  
split → [4,2] [3,1]  
[4,2] → [4] [2] → merge → [2,4]  
[3,1] → [1,3]  
merge [2,4] & [1,3] → [1,2,3,4]

### 💻 Code
```java
public static void mergeSort(int[] arr) {
    if (arr == null || arr.length <= 1) return;
    int[] tmp = new int[arr.length];
    mergeSortRec(arr, tmp, 0, arr.length - 1);
}

private static void mergeSortRec(int[] a, int[] tmp, int l, int r) {
    if (l >= r) return;
    int m = l + (r - l) / 2;
    mergeSortRec(a, tmp, l, m);
    mergeSortRec(a, tmp, m + 1, r);

    int i = l, j = m + 1, k = l;
    while (i <= m && j <= r) {
        if (a[i] <= a[j]) tmp[k++] = a[i++];
        else tmp[k++] = a[j++];
    }
    while (i <= m) tmp[k++] = a[i++];
    while (j <= r) tmp[k++] = a[j++];
    for (k = l; k <= r; k++) a[k] = tmp[k];
}
```

### ⏱ Complexity
- Time: O(n log n) all cases  
- Space: O(n) aux (merge buffer)  
- Stable: Yes  
- Use: reliable general-purpose sort, external sorting possible.

---