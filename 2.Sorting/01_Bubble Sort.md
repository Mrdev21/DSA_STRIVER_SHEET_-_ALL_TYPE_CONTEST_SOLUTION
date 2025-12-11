
## 1) Bubble Sort

### 🧠 Intuition (Hinglish)
Pair-wise adjacent elements ko compare karke **largest elements ko "bubble up"** kar dete hain end ki taraf. Har pass me biggest element place ho jata hai.

### ⚙️ Approach
- Repeat n-1 passes.
- Har pass me adjacent pairs (j, j+1) compare karo; agar arr[j] > arr[j+1] swap.
- After i-th pass last i elements sorted.

### 📝 Dry Run
arr = [4, 2, 3, 1]
Pass1: [2,4,3,1] → [2,3,4,1] → [2,3,1,4]
Pass2: [2,3,1,4] → [2,1,3,4] → [1,2,3,4]
Pass3: final sorted.

### 💻 Code (Java)
```java
public static void bubbleSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                int t = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = t;
            }
        }
    }
}
```

### ⏱ Complexity & Notes
- Time: O(n²) worst/avg/best (best can be improved with early exit)  
- Space: O(1)  
- Stable: Yes  
- Use: educational, small n, or nearly sorted with optimized version.

---

## 2) Optimized Bubble Sort (Early exit)

### 🧠 Intuition
Agar kisis pass me koi swap na ho to array already sorted — loop break kar do.

### ⚙️ Approach
Same as bubble, but maintain `swapped` flag each pass.

### 📝 Dry Run
arr = [1,2,3,4] → pass1 no swaps → break → done.

### 💻 Code
```java
public static void bubbleSortOptimized(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        boolean swapped = false;
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                int t = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = t;
                swapped = true;
            }
        }
        if (!swapped) break;
    }
}
```

### ⏱ Complexity
- Best: O(n) (already sorted)  
- Avg/Worst: O(n²)  
- Stable: Yes

---
