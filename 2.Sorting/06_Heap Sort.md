
### 🧠 Intuition
Build a max-heap from array, then repeatedly extract max (swap with end) and heapify reduced heap — results in sorted ascending array.

### ⚙️ Approach
- Build max-heap (heapify from n/2-1 down to 0).
- For end = n-1 down to 1: swap a[0] and a[end]; heapify(0, end).

### 📝 Dry Run
arr=[4,2,3,1]
build max-heap -> [4,2,3,1]
swap 4 and 1 -> [1,2,3,4], heapify first part -> final sorted.

### 💻 Code
```java
public static void heapSort(int[] arr) {
    int n = arr.length;
    for (int i = n / 2 - 1; i >= 0; i--) heapify(arr, n, i);
    for (int end = n - 1; end > 0; end--) {
        int t = arr[0]; arr[0] = arr[end]; arr[end] = t;
        heapify(arr, end, 0);
    }
}
private static void heapify(int[] a, int heapSize, int i) {
    int largest = i;
    int l = 2 * i + 1;
    int r = 2 * i + 2;
    if (l < heapSize && a[l] > a[largest]) largest = l;
    if (r < heapSize && a[r] > a[largest]) largest = r;
    if (largest != i) {
        int t = a[i]; a[i] = a[largest]; a[largest] = t;
        heapify(a, heapSize, largest);
    }
}
```

### ⏱ Complexity
- Time: O(n log n) worst/avg/best  
- Space: O(1) (in-place)  
- Not stable  
- Use: when O(1) extra is required and guaranteed O(n log n) needed.

---
